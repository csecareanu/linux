Each sequence contains two lines.\
The first line is the header which is denoted with a ‘>’ sign.\
The second line is the actual sequence data.

How many sequences do we have in our data set?\
Each sequence is represented by two lines:
-    the first line is a header (provides information about the sequence) which is denoted with a ‘>’ sign.
-    the second line is the actual sequence. But in this situation the sequence is represented by multiple lines, every line being of 80-90 characters long (head rrnDB-5.6_16S_rRNA.fasta)

Can use ‘wc’? (no because there are multiple lines for one sequence)\
We can use grep to get the number of sequences
```
grep ">" rrnDB-5.6_16S_rRNA.fasta | wc
77530  374262 7607109

77530 lines, 374262 words, 7607109 characters
```

Get only the number of lines
```
grep ">" rrnDB-5.6_16S_rRNA.fasta | wc -l
77530
```
or
```
grep -c ">" rrnDB-5.6_16S_rRNA.fasta
77530
```

### Analysing the rrnDB.align table

`grep ">" data/v19/rrnDB.align | head -1`

`|` - demarcate different fields

```
Escherichia_coli|GCF_000599665.1|NZ_CP007392.1|Chromosome__ANONYMOUS|143933..145488_+
```

`Escherichia_coli` - the organism name\
`GCF_....` - assembly accession from genbank\
`NZ_...` - the refseq accession number from genbank\
`Chromosome__ANONYMOUS` - information about which chromosome or which genetic elemenent it came from in the genome\
`143933..145488_+` - the coordonates in the genome where the gene was identified from
