# Grep


## Syntax

### Count and print the number of lines
`grep -c <file name>`
```
grep -c  ">" rrnDB-5.6_16S_rRNA.fasta
77530
```

You can get the same result using 'wc'
```
grep ">" rrnDB-5.6_16S_rRNA.fasta | wc -l
77530
```

### To show you the lines before your matches, you can add -B to your grep.
```
grep -B <number_of_lines_before> 'keyword' /path/to/file.log
```

### To show the log lines that match after the keyword, use the -A parameter.
```
grep -A <number_of_lines_after> 'keyword' /path/to/file.log
```
```
grep "8914..10469" rrnDB-5.6_16S_rRNA.fasta
>Bacillus mycoides|GCF_008274925.1|NZ_CP031071.1|Chromosome: ANONYMOUS|8914..10469 +
```
```
grep -A 1 "8914..10469" rrnDB-5.6_16S_rRNA.fasta
>Bacillus mycoides|GCF_008274925.1|NZ_CP031071.1|Chromosome: ANONYMOUS|8914..10469 +
TTTATTGGAGAGTTTGATCCTGGCTCAGGATGAACGCTGGCGGCGTGCCTAATACATGCAAGTCGAGCGAATGGATTAA
```
