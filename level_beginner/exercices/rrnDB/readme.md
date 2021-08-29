https://www.youtube.com/watch?v=gPX7d3BcZpg&t=113s


Each sequence contains two lines.\
The first line is the header which is denoted with a ‘>’ sign.\
The second line is the actual sequence data.

How many sequences do we have in our data set?\
Each sequence is represented by two lines:
-    the first line is a header (provides information about the sequence) which is denoted with a ‘>’ sign.
-    the second line is the actual sequence. But in this situation the sequence is represented by multiple lines, every line being of 80-90 characters long (head rrnDB-5.6_16S_rRNA.fasta)

Can use ‘wc’? (no because there are multiple lines for one sequence)\
We can use grep to get the number of sequences (77530 lines, 77530 words, 7607109 characters):
```sh
grep ">" data/v19/rrnDB.align | wc
77530  77530 7607109
```

Display only the number of lines (instead of no of lines, words and characters as above)
```sh
grep ">" data/v19/rrnDB.align | wc -l
77530
```
or
```sh
grep -c ">" data/v19/rrnDB.align
77530
```

### Analysing the rrnDB.align table

#### 1. Check how the data looks like
```
grep ">" data/v19/rrnDB.align | head -1
Escherichia_coli|GCF_000599665.1|NZ_CP007392.1|Chromosome__ANONYMOUS|143933..145488_+
```
`|` - demarcate different fields
`Escherichia_coli` - the organism name\
`GCF_....` - assembly accession from genbank\
`NZ_...` - the refseq accession number from genbank\
`Chromosome__ANONYMOUS` - information about which chromosome or which genetic elemenent it came from in the genome\
`143933..145488_+` - the coordonates in the genome where the gene was identified from

#### 2. Using cut to identify fields and pull that field of the full listing of all these lines
Instead of using `head` (as above) we will use `cut`

__Listing the genus name for every sequence in our data set.__ \
Each sequence can occur multiple times in each genome. So we can have multiple names for each genome.
We want to count the number of genomes in the data set.\
So we need first to get the unique genome assemblies.


We need the `GCF` field.

We will use `head` because there are 76000 sequences and there will be printed to much data to the screen (`head` will print only the first 10 entries, which is enough for us to see how the data looks like)
```sh
grep ">" data/v19/rrnDB.align | cut -f 1 -d "_" | head

>Escherichia
>Escherichia
>Escherichia
>Escherichia
>Morganella
>Escherichia
>Proteus
>Escherichia
>Escherichia
>Streptococcus

```

To get the assembly numbers:
```sh
grep ">" data/v19/rrnDB.align | cut -f 2 -d "|" | head

GCF_000599665.1
GCF_000599665.1
GCF_000599665.1
GCF_000599665.1
GCF_902387845.1
GCF_000599665.1
GCF_000069965.1
GCF_000599665.1
GCF_000599665.1
GCF_000253155.1
```

I don't have my taxonomic information. To get both the taxonomic name as well as the assembly name:
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | head

>Escherichia_coli|GCF_000599665.1
>Escherichia_coli|GCF_000599665.1
>Escherichia_coli|GCF_000599665.1
>Escherichia_coli|GCF_000599665.1
>Morganella_morganii|GCF_902387845.1
>Escherichia_coli|GCF_000599665.1
>Proteus_mirabilis_HI4320|GCF_000069965.1
>Escherichia_coli|GCF_000599665.1
>Escherichia_coli|GCF_000599665.1
>Streptococcus_oralis_Uo5|GCF_000253155.1
```

This is a unique combination which I can then:
``` sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | uniq | head

>Escherichia_coli|GCF_000599665.1
>Morganella_morganii|GCF_902387845.1
>Escherichia_coli|GCF_000599665.1
>Proteus_mirabilis_HI4320|GCF_000069965.1
>Escherichia_coli|GCF_000599665.1
>Streptococcus_oralis_Uo5|GCF_000253155.1
>Proteus_mirabilis_HI4320|GCF_000069965.1
>Escherichia_coli|GCF_000599665.1
>Streptococcus_oralis|GCF_001983955.1
>Proteus_mirabilis_HI4320|GCF_000069965.1
```

The problem with this is that unique only compares succesive rows of data. So what we need to do instead is we first need to sort the data and then unique it.\
So we can see now there are multiple genomes that we have sorted it:
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | head

>'Catharanthus_roseus'_aster_yellows_phytoplasma|GCF_004214875.1
>'Catharanthus_roseus'_aster_yellows_phytoplasma|GCF_004214875.1
>'Nostoc_azollae'_0708|GCF_000196515.1
>'Nostoc_azollae'_0708|GCF_000196515.1
>'Nostoc_azollae'_0708|GCF_000196515.1
>'Nostoc_azollae'_0708|GCF_000196515.1
>Acaryochloris_marina_MBIC11017|GCF_000018105.1
>Acaryochloris_marina_MBIC11017|GCF_000018105.1
>Acetoanaerobium_sticklandii|GCF_000196455.1
>Acetoanaerobium_sticklandii|GCF_000196455.1
```

So we have uniqued the genomes:
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | uniq | head

>'Catharanthus_roseus'_aster_yellows_phytoplasma|GCF_004214875.1
>'Nostoc_azollae'_0708|GCF_000196515.1
>Acaryochloris_marina_MBIC11017|GCF_000018105.1
>Acetoanaerobium_sticklandii|GCF_000196455.1
>Acetobacter_aceti|GCF_002005445.1
>Acetobacter_ascendens|GCF_001766235.1
>Acetobacter_ascendens|GCF_001766255.1
>Acetobacter_ascendens|GCF_002173775.1
>Acetobacter_orientalis|GCF_003966365.1
>Acetobacter_oryzifermentans|GCF_001628715.1
```

Now we need to know for the unique genomes how many genera do we have.\
We will get only the genus names represented:
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | uniq | cut -f 1 -d "_" | head

>'Catharanthus
>'Nostoc
>Acaryochloris
>Acetoanaerobium
>Acetobacter
>Acetobacter
>Acetobacter
>Acetobacter
>Acetobacter
>Acetobacter
```

We will sort them again and make them unique:
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | uniq | cut -f 1 -d "_" | sort | uniq | head

>'Catharanthus
>'Nostoc
>Acaryochloris
>Acetoanaerobium
>Acetobacter
>Acetobacteraceae
>Acetobacterium
>Acetohalobium
>Acetomicrobium
>Acholeplasma
```

I don't want only the listing of uniques, I want to count the number of uniques.\
I want to count the number of times each unique sequence appears.
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | uniq | cut -f 1 -d "_" | sort | uniq -c | head

      1 >'Catharanthus
      1 >'Nostoc
      1 >Acaryochloris
      1 >Acetoanaerobium
     28 >Acetobacter
      2 >Acetobacteraceae
      1 >Acetobacterium
      1 >Acetohalobium
      1 >Acetomicrobium
      7 >Acholeplasma
```
We can see that _Acetobacter_ is represented by 28 genomes in rrndb whereas _Acetoanaerobium_ only occurs in one genome.


Get the most frequent genera in the dataset
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | uniq | cut -f 1 -d "_" | sort | uniq -c | sort -n -r | head

 979 >Escherichia
 787 >Salmonella
 640 >Bordetella
 636 >Streptococcus
 604 >Bacillus
 597 >Staphylococcus
 536 >Pseudomonas
 482 >Klebsiella
 394 >Lactobacillus
 272 >Campylobacter
```

To get the number of different generas:
```sh
grep ">" data/v19/rrnDB.align | cut -f 1,2 -d "|" | sort | uniq | cut -f 1 -d "_" | sort | uniq -c | sort -n -r | wc -l

1313
```

### Make an unique identifier for each organism (each sequence and each genome in the dataset)

We want to create a table to have each row represented by a different sequence (a unique sequence), and each colomn be genomes that that sequence was found in 
and the values in the table then being the number of times each sequence is found in each genome.

We need a grouping file that tells us which sequence belongs to each genus or each group.\
To get there we need to simplify the header a littel bit to identify what is the unique identifier that we can use to represent each sequence.

###  Count the frequency of sequences across genomes
#### Create a table indicating number of times each unique sequence appears across genomes

* Create a grouping file to relate each sequence to each genome.
* Identifiy the unique sequences in the database
* Count the number of times each unique sequence occurs across the genomes.


**This is how a squence looks like in database:**
```sh
>Escherichia_coli|GCF_000599665.1|NZ_CP007392.1|Chromosome__ANONYMOUS|143933..145488_+
```

We have 77530 sequences. So whne we create those identifiers that's the number that we are going to need to have.
```sh
$ grep -c ">" data/v19/rrnDB.align
77530
```

If we would create an identifier based on genus name we only get out 1313 genera.\
That is not going to be a unique identifier.
```sh
$ grep ">" data/v19/rrnDB.align | cut -f 1 -d "_" | sort | uniq | wc -l
1313
```

Let's change this instead to be the _gcf_ and the _um_ coordonates because hopefully the genome (the assembly) as well as the coordinates of where it occurs will put us in good shape. 
```sh
$ grep ">" data/v19/rrnDB.align | cut -f 2,5 -d "|" | sort |  uniq | wc -l
77526
```
It appears that this is also not a perfect identifier (77526 < 77530).

Let's check for duplicates (we could have duplicates with the assembly as well as the coordonates):
```sh
$ grep ">" data/v19/rrnDB.align | cut -f 2,5 -d "|" | sort |  uniq -d

GCF_001576595.1|1..1471_+
GCF_001685625.1|1..1471_+
GCF_002706325.1|1..1471_+
GCF_003324715.1|1..1471_+
```
There is four assemblies that have two 16s copies that start at position one and go to 1470.\
Let's check what those are are and who those are coming from:
```
$ grep "GCF_001576595.1" data/v19/rrnDB.align

>Rhodobacter_sphaeroides|GCF_001576595.1|NZ_CP012960.1|Chromosome__1|1..1471_+
>Rhodobacter_sphaeroides|GCF_001576595.1|NZ_CP012961.1|Chromosome__2|1..1471_+
>Rhodobacter_sphaeroides|GCF_001576595.1|NZ_CP012961.1|Chromosome__2|33674..35144_+
```
The first one (_GCF_001576595.1_) is coming from _Rhodobacter_sphaeroides_ and there is two chromosomes. _Rhodobacter_sphaeroides_ has two chromosomes in its genome and the way this group that sequenced the genomes numbered the bases is that for both chromosomes. They started with a 16s gene.

Check if it is tru with some of these others:
```
$ grep "GCF_003324715.1" data/v19/rrnDB.align

>Rhodobacter_sphaeroides_2.4.1|GCF_003324715.1|NZ_CP030271.1|Chromosome__1|1..1471_+
>Rhodobacter_sphaeroides_2.4.1|GCF_003324715.1|NZ_CP030272.1|Chromosome__2|1..1471_+
>Rhodobacter_sphaeroides_2.4.1|GCF_003324715.1|NZ_CP030272.1|Chromosome__2|33674..35144_+
```

It appears that the assembly and the coordinates (_gcf_ and the _um_) are not sufficient that will also have to include the _refseq id_.\
So we will need three fields.

```sh
$ grep ">" data/v19/rrnDB.align | cut -f 2,3,5 -d "|" | sort |  uniq | wc -l

77530
```
We got 77530 unique identifiers as well as 77530 sequences. So we have got one-to-one correspondence of our identifiers.


Now we will use `sed` to extract fields 2,3,5 to create a new file in that the first comlumn will be fileds 2,3,5 (the sequence identifier) and then the second column will be filed 2, the genome assembly that 3 and 5 the different copies correspond to.\
Then we will modify the alignments in file so they have the updated names and so then with that grouping file linking our unique sequence identifier to the genome they came from we can than count the number of times the unique sequences show up in those fields.\
We will do that in our _count_unique_seqs.sh_ file.

```sh
#!/usr/bin/env bash

TARGET=data/v19/
```
