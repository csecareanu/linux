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
