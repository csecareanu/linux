# WC

__WC__ stands for _Word Count_

## Syntax

### Print the number of lines, words and characters in the file
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
Uniq is the tool that helps to detect the __adjacent__ duplicate lines and also deletes the duplicate lines.

## Parameters
* without parameters: return the unique lines
* -c: the number of times each unique line appears
```
$cat kt.txt
Output:
I love music.
I love music.
I love music of Kartik.
I love music.
I love music of Kartik.

$ cat kt.txt | sort | uniq -c
Output:
2 I love music of Kartik.
3 I love music.


$ cat kt.txt | sort | uniq
Output:
I love music of Kartik.
I love music.
```

Because uniq filters out the __adjacent__ matching lines, if you want to remove all matching lines, you have to sort first the input:
```
$cat kt.txt
Output:
I love music.
I love music.
I love music of Kartik.
I love music.
I love music of Kartik.

$ cat kt.txt | uniq
Output:
I love music.
I love music of Kartik.
I love music.
I love music of Kartik.

$ cat kt.txt | sort | uniq
Output:
I love music of Kartik.
I love music.
```
* -d: It only prints the repeated lines (prints just one line for more reapeating lines) and not the lines which aren’t repeated.\
It is usefull to check if there are duplicate lines in a file.

* -i: By default, comparisons done are case sensitive but with this option case insensitive comparisons can be made.

# SORT

## Parameters
* -f, --ignore-case
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

**Take care that if you use sort for numerical values without -n the result will be orderd by the ASCII value of the characters:**
```
$ sort file1.txt
Output :
15
200
39
50
89
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
