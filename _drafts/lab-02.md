Lab 2 - HPC and Command Line BLAST
==================================


Goals:

Introduce HPC. Short lecture on using a cluster

- Head node vs compute nodes
- Submitting jobs
- Storage
- Moving files on and off
- modules

BLAST tutorial

- start with getting aa sequence from your protein and blasting against (mouse? human? zebrafish?)
- Discuss automation
- follow ANGUS. blast mouse against zebrafish
	- ideally replace with corn against arabidopsis, or human against mouse
	- submit 1-core jobs
- Discuss output
	- human readable vs table
-


### [SLURM](https://slurm.schedmd.com/)


### Batch Jobs
  * sbatch

### Interactive
  * srun

### modules



## Running BLAST from the Bash Terminal

First, create a directory for storing all of the files used in today's lab exercise:


    mkdir lab-02
    cd lab-02


Next, we need to add the **BLAST** program to our environment variable PATH (**$PATH**) by loading the module:

    module load blast

Now, let's grab the mouse and zebrafish RefSeq
protein data sets from NCBI, and put them in our work directory. We'll
use `curl` to download the files:

    curl -O ftp://ftp.ncbi.nih.gov/refseq/M_musculus/mRNA_Prot/mouse.1.protein.faa.gz
    curl -O ftp://ftp.ncbi.nih.gov/refseq/M_musculus/mRNA_Prot/mouse.2.protein.faa.gz
    curl -O ftp://ftp.ncbi.nih.gov/refseq/M_musculus/mRNA_Prot/mouse.3.protein.faa.gz

    curl -O ftp://ftp.ncbi.nih.gov/refseq/D_rerio/mRNA_Prot/zebrafish.1.protein.faa.gz

> **Thought Questions:**
* What does the -O option specify for the curl command?
  * How can you find out?

Let's look at the files in the current directory:

    ls -l

You should see output similar to:

    smhigdon@farm:~/BIT150/lab-02$ ls -l
    total 29116
    -rw-rw-r-- 1 smhigdon smhigdon  3433688 Sep 14 23:54 mouse.1.protein.faa.gz
    -rw-rw-r-- 1 smhigdon smhigdon  5451738 Sep 14 23:54 mouse.2.protein.faa.gz
    -rw-rw-r-- 1 smhigdon smhigdon  6898191 Sep 14 23:54 mouse.3.protein.faa.gz
    -rw-rw-r-- 1 smhigdon smhigdon 14021957 Sep 14 23:54 zebrafish.1.protein.faa.gz

All four of the files are FASTA protein files (that's what the .faa
suggests) that are compressed with `gzip` (that's what the .gz means).

Let's uncompress them:


    gunzip *.faa.gz

and let's look at the first few sequences in the file:

    head mouse.1.protein.faa

These are protein sequences in **FASTA** format. FASTA format is something
many of you have probably seen in one form or another -- it's pretty
ubiquitous. FASTA files are text files that contain records; each record
starts with a line beginning with a `>`, and then contains one or more
lines of sequence text. The line beginning with a `>` is called the **header**.

Let's take the first two sequences we just saw using the head command and save them to a file. We can achieve this by using output redirection with `>`, which says:
> "take all output from the previous command and store it in the following file."

    head -11 mouse.1.protein.faa > mm-first.fa

So now, for example, you can do `cat mm-first.fa` to see the contents of
that file (or `less mm-first.fa`).

Now let's **BLAST** these two sequences against the entire zebrafish
protein data set. First, we need to tell BLAST that the zebrafish
sequences are:
* (a) a database, and
* (b) a protein database.
That's done by calling `makeblastdb`:

    makeblastdb -in zebrafish.1.protein.faa -dbtype prot

Next, we call **BLAST** to do the search:

    blastp -query mm-first.fa -db zebrafish.1.protein.faa

This should run pretty quickly, but you're going to get a lot of output!!
To save it to a file instead of watching it go past on the screen,
ask BLAST to save the output to a file that we'll name `mm-first.x.zebrafish.txt`:

    blastp -query mm-first.fa -db zebrafish.1.protein.faa -out mm-first.x.zebrafish.txt

Now you can `page` through this file at your leisure by typing:

    less mm-first.x.zebrafish.txt

(Type spacebar to move down, and 'q' to get out of paging mode.)

-----

Let's do some more sequences (this one will take a little longer to run):

    head -500 mouse.1.protein.faa > mm-second.fa
    blastp -query mm-second.fa -db zebrafish.1.protein.faa -out mm-second.x.zebrafish.txt

This will compare the first 83 sequences.  You can look at the output file with:

    less mm-second.x.zebrafish.txt

(again, type 'q' to get out of paging mode.)


> **Thought Questions**:
  * You can execute multiple commands at a time;
  * You might see a warning -
        `Selenocysteine (U) at position 310 replaced by X`
      * what does this mean?
  * Why did it take longer to BLAST `mm-second.fa` than `mm-first.fa`?

### Items for discussion:

* `blastp` options and -help.
* command line options, more generally - why so many?
* automation rocks!
* when you shut down, you lose all your data
* what computer(s) is this all happening on?

## Visualizing BLAST output in RStudio
