# Grep

__Grep__ stands for _Global Regular Expression Print_


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
