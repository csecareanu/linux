Each sequence contains two lines.\
The first line is the header which is denoted with a ‘>’ sign.\
The second line is the actual sequence data.

How many sequences do we have in our data set?\
Each sequence is represented by two lines:
-    the first line is a header (provides information about the sequence) which is denoted with a ‘>’ sign.
-    the second line is the actual sequence. But in this situation the sequence is represented by multiple lines, every line being of 80-90 characters long (head rrnDB-5.6_16S_rRNA.fasta)

Can use ‘wc’? (no because there are multiple lines for one sequence)\
We can use grep to get the number of sequences (77530 lines, 77530 words, 7607109 characters):
```
grep ">" data/v19/rrnDB.align | wc
77530  77530 7607109
```

Display only the number of lines (instead of no of lines, words and characters as above)
```
grep ">" data/v19/rrnDB.align | wc -l
77530
```
or
```
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
```
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
```
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
```
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
``` 
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
```
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
```
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
```
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
```
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
I want to count the number of times each unique sequence appears.\
```
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
