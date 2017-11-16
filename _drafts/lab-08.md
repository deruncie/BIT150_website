Lab 8: Microbial Genome Assembly
================================

## Overview

* [Downloading Sequence Data](# downloading-sequence-data)
* [FastQC](# FastQC)

## Downloading Sequence Data


First, let's create a set of nested directories to work in for today's lab. We will put all
files and subfolders associated with this lab into the directory ``~/lab_08``

    mkdir -p ~/lab_08/{data,analysis}

Next, let's download the fastq files containing the raw nucleotide reads for the Escherichia coli O104:H4 str. TY-2482 genome from the [ENA](http://www.ebi.ac.uk/ena):

    cd lab_08/data
    curl -O -L ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR292/SRR292770/SRR292770_1.fastq.gz
    curl -O -L ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR292/SRR292770/SRR292770_2.fastq.gz

Let's make sure we downloaded uncorrupted files by using the unix command md5sum:

    md5sum SRR292770_1.fastq.gz SRR292770_2.fastq.gz

You should see this:

    89efdc20d85c4a16cdee9b223f7c57c6  SRR292770_1.fastq.gz
    83f502a4e75aeaa601fa615afd4a78c9  SRR292770_2.fastq.gz

Now if you type:

    ls -l

you should see something like:

     total 346936
     -rw-rw-r-- 1 ubuntu ubuntu 169620631 Oct 11 23:37 SRR1976948_1.fastq.gz
     -rw-rw-r-- 1 ubuntu ubuntu 185636992 Oct 11 23:38 SRR1976948_2.fastq.gz

One problem with these files is that they are writeable - by default, UNIX
makes things writeable by the file owner. This poses an issue with creating
typos or errors in raw data. Let's fix that before we go on any further:

    chmod u-w *

We'll talk about what these files are below.

### Copying data into a working directory


Recall that above we created two separate directories below the `lab_06` directory: `data` and `analysis`.

It is common to store data files in directories that are separate from the directory in which you store output from analysis with bioinformatic programs.

Rather than typing the absolute or relative path to the data files, let's create a "virtual copy" of the data in the **analysis** directory
by creating a symbolic link to each file:

    cd ../analysis
    ln -s ../data/* .

These are FASTQ files -- let's take a look at them:

    less SRR1976948_1.fastq.gz

**(use the spacebar to scroll down, and type 'q' to exit 'less')**

> **Thought Questions:**
  * Where does the filename come from?
  * Why are there `_1` and `_2` in the file names?
  * What does each of the four lines of a read indicate?
  * Links:
    * [FASTQ Format](http://en.wikipedia.org/wiki/FASTQ_format)

## Barcode Trimming and Read Quality Assessment/Control

Although raw sequence read files deposited in the SRA database are heavily screened, it is still a good idea to verify that the reads being assembled are free of Illumina adapter sequences, indexing barcode sequences and low quality base calls. We will use two programs to conduct a Quality Assessment and impose Quality Control __(QA/QC)__.

### FastQC

We're going to use [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) to
summarize the data. **FastQC** is already installed on our HPC as a module -
let's activate it and run the program on the two fastq files we downloaded:

    module load fastqc
    fastqc SRR292770_1.fastq.gz
    fastqc SRR292770_2.fastq.gz

Now type 'ls':

    ls -d *fastqc.zip*

to list the files, and you should see:

    SRR1976948_1_fastqc.zip
    SRR1976948_2_fastqc.zip

Inside each of the fatqc directories you will find reports from the fastqc. You can download these files using your Jupyter Notebook console, if you like;
or you can look at these copies of them:

* `SRR1976948_1_fastqc/fastqc_report.html <http://2017-ucsc-metagenomics.readthedocs.io/en/latest/_static/SRR1976948_1_fastqc/fastqc_report.html>`__
* `SRR1976948_2_fastqc/fastqc_report.html <http://2017-ucsc-metagenomics.readthedocs.io/en/latest/_static/SRR1976948_2_fastqc/fastqc_report.html>`__

> **Thought Questions:**
* What should you pay attention to in the FastQC report?
* Which is "better", file 1 or file 2? Which factors contribute most to high read quality?
* Links:
  * [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
  * [FastQC tutorial video](http://www.youtube.com/watch?v=bz93ReOv87Y)

There are several caveats about FastQC - the main one is that it only
calculates certain statistics (like duplicated sequences) for subsets
of the data (e.g. duplicate sequences are only analyzed for the first

### Trimmomatic

Now we're going to do some sequence trimming.  We'll be using
[Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic), which
(as with fastqc) is already installed on the cluster as a loadable module.

The first thing we'll need to do is load the module and download the sequences of the adapters to trim off:

    module load trimmomatic
    curl -O -L http://dib-training.ucdavis.edu.s3.amazonaws.com/mRNAseq-semi-2015-03-04/TruSeq2-PE.fa

Now, to run Trimmomatic:

     TrimmomaticPE R1.fastq.gz \
                   R22.fastq.gz \
                   R1.qc.fq.gz s1_se \
                   R2.qc.fq.gz s2_se \
                   ILLUMINACLIP:TruSeq2-PE.fa:2:40:15 \
                   LEADING:2 TRAILING:2 \
                   SLIDINGWINDOW:4:2 \
                   MINLEN:25

You should see output that looks like this:

     ...
     Input Read Pairs: 1000000 Both Surviving: 885734 (88.57%) Forward Only Surviving: 114262 (11.43%) Reverse Only Surviving: 4 (0.00%) Dropped: 0 (0.00%)
     TrimmomaticPE: Completed successfully

> **Thought Questions:**
* How do you figure out what the parameters mean?
* How do you figure out which parameters to use?
* Which adapters do you use?
* What version of Trimmomatic are we using here? (And FastQC?)
* Do you think parameters are different for RNAseq and genomic data sets?
* What's with these annoyingly long and complicated filenames?
* Why are we running R1 and R2 together?

* For a discussion of optimal trimming strategies, see [MacManes, 2014](http://journal.frontiersin.org/Journal/10.3389/fgene.2014.00013/abstract)
-- it's about RNAseq but similar arguments should apply to microbial genome
assembly.

* Links:
  * `Trimmomatic <http://www.usadellab.org/cms/?page=trimmomatic>`__

### FastQC Again

Run FastQC again on the trimmed files::

   fastqc R1.qc.fq.gz
   fastqc R2.qc.fq.gz

And now view my copies of these files:

* `R1.qc_fastqc/fastqc_report.html <http://2016-metagenomics-sio.readthedocs.io/en/work/_static/SRR1976948_1.qc_fastqc/fastqc_report.html>`__
* `R2.qc_fastqc/fastqc_report.html <http://2016-metagenomics-sio.readthedocs.io/en/work/_static/SRR1976948_2.qc_fastqc/fastqc_report.html>`__

Let's take a look at the output files::

   less R1.qc.fq.gz

(again, use spacebar to scroll, 'q' to exit less).

> Thought Questions:
* Is the quality trimmed data "better" than before?
* Does it matter that you still have adapters?

## Genome Assembly

There are two main strategies for genome assembly using DNA sequencing reads, __*de novo* assembly__ and __reference based mapping__. Among the algorithm types used to carry out __*de novo* assembly__, the two most common strategies are *De Bruijn Graph Assembly* and *Overlap Layout Consensus*. For more information on the differences between these two, here are some papers:

* [Li et al., 2012](https://www.ncbi.nlm.nih.gov/pubmed/22184334)
* [Zerbino et al., 2008](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2336801/)
* [Miller et al., 2010](http://www.sciencedirect.com/science/article/pii/S0888754310000492)

### MEGAhit

To assemble the *E. coli* genome reads we have been working with, we will use **MEGAhit**, which is an ultra-fast short read assembly program that takes a *De Bruijn Graph* approach.

* The paper can be found [here](https://www.ncbi.nlm.nih.gov/pubmed/25609793)
* [MEGAhit source code](https://github.com/voutcn/megahit)

We want to assemble the trimmed reads that were generated after running trimmomatic. In the same directory, run the following commands:

```
module load megahit
megahit -1 SRR292770.R1.trim.fq -2 SRR292770.R2.trim.fq -o megahit_2482 -t 4
```

## Assembly Quality

After a *de novo* assembly has been generated, a question that may come to mind is...

> How good is my assembly, and how can I measure the quality?

While there are many metrics used, two of the most common assembly metrics are **N50** and **L50**.

The __N50__ value of an assembly is defined as the length of the smallest contig within a subset of the largest assembled contigs whose sum represents 50 percent of the total number of bases assembled.

The __L50__ value is then defined as the the minimum number of contigs whose length sum is equal to the N50 value.

To assess these metrics along with many others, we will use the program below:

### QUAST: QUality ASsessment Tool for Genome Assemblies
* Click [here](http://quast.sourceforge.net/) for more information on QUAST.
* The paper can be found [here](https://academic.oup.com/bioinformatics/article/29/8/1072/228832/QUAST-quality-assessment-tool-for-genome).
* [QUAST source code](https://github.com/ablab/quast)

To evaluate the quality of your MEGAhit assembly, run the following commands:

```
module load bio
quast.py megahit_2482/final.contigs.fa -o quast_2482
```

> Notice that we loaded a module called `bio` rather than one with a name specific to the program we want to run. Loading this custom module will actually add many commonly used bioinformatics program binary files to your __$PATH__ so that you can easily use them in your interactive shell. To check out some of these programs, type the following commands after you have loaded the `bio` module:
* `pip list` to view python modules.
* `conda list` to view [bioconda](https://bioconda.github.io/) modules.

Running `QUAST` on our input file containing the TY-2482 assembly contigs from MEGAhit actually generated many files nested within the new directory, `quast_2482`. Let's have a look at the report in tab-separated-value __(tsv)__ format:
```
less -S quast_2482/report.txt
```
You should see something very similar to the following:

```
All statistics are based on contigs of size >= 500 bp, unless otherwise noted (e.g., "# contigs (>= 0 bp)" and "Tota

Assembly                    final.contigs
# contigs (>= 0 bp)         719
# contigs (>= 1000 bp)      369
# contigs (>= 5000 bp)      211
# contigs (>= 10000 bp)     151
# contigs (>= 25000 bp)     60
# contigs (>= 50000 bp)     19
Total length (>= 0 bp)      5210009
Total length (>= 1000 bp)   5065636
Total length (>= 5000 bp)   4702255
Total length (>= 10000 bp)  4259183
Total length (>= 25000 bp)  2792051
Total length (>= 50000 bp)  1285159
# contigs                   463
Largest contig              143131
Total length                5130667
GC (%)                      50.49
N50                         30790
N75                         14105
L50                         52
L75                         117
# N's per 100 kbp           0.00

```

> **Thought Questions:**
* Why do we have so many contigs?
* What would be needed to produce an assembly with a total number of contigs under, say 10...or perhaps even a **single contig scaffold**?

## Putting it all together: sbatch scripts

Running through this entire process step by step is great for learning how everything works, but what if you need to assemble many different genomes in a short amount of time? A more efficient way to conduct this entire computational analysis is to write a shell script that can be interpreted by SLURM. Let's take a look at the example script below:

### sbatch script

    #!/bin/bash -l

    #SBATCH -p high
    #SBATCH -D /home/smhigdon/BIT150
    #SBATCH -o /home/smhigdon/BIT150/slurm-log/MEGA_B150-stdout-%j.txt
    #SBATCH -e /home/smhigdon/BIT150/slurm-log/MEGA_B150-stderr-%j.txt
    #SBATCH -J MEGA_B150
    #SBATCH -N 1
    #SBATCH -n 8
    #SBATCH --mem=16000
    #SBATCH -t 24:00:00
    #SBATCH --mail-type=END,FAIL
    #SBATCH --mail-user=smhigdon@ucdavis.edu

    # Name: megahit_BIT150.sh
    # Created by: Shawn Higdon
    # creation date: April 25, 2017
    # This is a pipeline for the assembly of Illumina PEx150 reads for individual microbial genomes.

    ## Load the required software module

    module load bio

    OUTPUT_FOLDER=lab_08
    mkdir -p $OUTPUT_FOLDER

    #Begin sample processing loop

    for R1 in $OUTPUT_FOLDER/*_1.fastq.gz
    do

    # Step One: Identify basename of paired end read files from the same genome library sample

    echo $R1
    R2=`echo $R1 | sed 's/_1/_2/'`
    bname=`echo $R1 | sed 's/_1.\+//'`
    bname=`basename $bname`
    echo $R2
    echo $bname
    samplename=$(echo $bname | cut -d_ -f1)
    echo $samplename

    SAMPLE_FOLDER=$OUTPUT_FOLDER/$samplename
    mkdir -p $SAMPLE_FOLDER

    # Step Two: Preprocess the raw paired end read files for each genome library using Trimmomatic

    trimmomatic PE -threads 8 $R1 $R2 $SAMPLE_FOLDER/$samplename.R1.trim.fq $SAMPLE_FOLDER/$samplename.orphan_R1.fq $SAMPLE_FOLDER/$samplename.R2.trim.fq $SAMPLE_FOLDER/$samplename.orphan_R2.fq ILLUMINACLIP:lab_08/TruSeq-PE-all.fa:2:40:15 LEADING:2 TRAILING:2 SLIDINGWINDOW:4:15 MINLEN:30

    # Step Three: Assemble the preprocessed read files for each sample using MEGAHIT

    MEGAHIT_FOLDER=$SAMPLE_FOLDER/megahit
    megahit -1 $SAMPLE_FOLDER/$samplename.R1.trim.fq -2 $SAMPLE_FOLDER/$samplename.R2.trim.fq -o $MEGAHIT_FOLDER -t 8
    # Step Four: Use Quast to determine the quality of the MEGAHIT assembly

    QUAST_FOLDER=$SAMPLE_FOLDER/quast
    quast.py $MEGAHIT_FOLDER/final.contigs.fa -o $QUAST_FOLDER

    done

    hostname
    export OMP_NUM_THREADS=$SLURM_NTASKS
    module load benchmarks
    stream

----



> [Project 3](project-3.md)
