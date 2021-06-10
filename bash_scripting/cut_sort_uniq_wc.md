# WC

__WC__ stands for _Word Count_

## Syntax

### Print the lines, words and the number of characters in the file
`wc <file name>` 

```
wc rrnDB-5.6_16S_rRNA.fasta
1625861   1922593 128985368 rrnDB-5.6_16S_rRNA.fasta
```

### Print only the number of lines in the file
` wc -l <file name>` 
```
 wc -l rrnDB-5.6_16S_rRNA.fasta
 1625861 rrnDB-5.6_16S_rRNA.fasta
```


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
