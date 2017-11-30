## Bonus

### Customizing HMM Searches

We learned about HMMs and how __PROKKA__ incorporates hmm searches into the prokaryotic genome annotation pipeline using pre-computed pHMM databases. But what if the genes you are interested in are not present in the databases, or you want to search for the presence of genes that have less general cellular functions? There are two options for this:

1. Modify the HMM databases called upon by PROKKA when annotation is performed. To do this, you just need to add the pHMM database file of choice to the path where PROKKA looks for databases. This is a bit more challenging if you do not have user permissions to change files on farm modules. For information on how to customize the HMM databases called by PROKKA, see the [PROKKA Github repository](https://github.com/tseemann/prokka){:target="_blank"}.
2. Another option is to use `HMMscan`, which is a program built into the `HMMER` software package, to query the __multi-fasta amino acid__ sequence files that were output by PROKKA against your pHMM library of choice. The strategy here is to generate a general genome annotation using PROKKA with the default databases, followed by a quick scan of the predicted protein coding features using `Hmmscan`
```
module load hmmer
```

```
ln -s /group/BIT150/Lab_9/cazy_scan.sh .
ln -s /group/BIT150/Lab_9/hmmscan-parser.sh .
```
