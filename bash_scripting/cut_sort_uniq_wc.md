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


# CUT
You can cut based on a delimiter found, or based on number of characters, or based on number of bytes.

## Parameters
* -f: which field you want
* -d: the delimiter used to define fields

# UNIQ

## Parameters
* default: return the unique lines
* -c: the number of times each unique line appears

# SORT

## Options
* -r sorting in reverse order
* -n: numerical sort
```
$ cat > file1.txt
50
39
15
89
200

$ sort -n file1.txt
Output :
15
39
50
89
200

$ sort -n -r file1.txt
Output :
200
89
50
39
15
```
* -k: which field to sort on
```
$ cat employee.txt
manager  5000
clerk    4000
employee  6000
peon     4500
director 9000
guard     3000

sort -k 2 -n employee.txt
guard    3000
clerk    4000
peon     4500
manager  5000
employee 6000
director 9000
```
* -t: the delimiter to define fields based on (default is a space character)
```
cat employee.txt
5000:manager:1
4000:clerk:4
6000:employee:3
4500:peon:2

sort -k 2 -t ':' -r employee.txt
4500:peon:2
5000:manager:1
6000:employee:3
4000:clerk:4

$ sort -k 2 -t ':' employee.txt
4000:clerk:4
6000:employee:3
5000:manager:1
4500:peon:2

$ sort -k 3 -n -t ':' employee.txt
5000:manager:1
4500:peon:2
6000:employee:3
4000:clerk:4

```
