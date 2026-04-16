# Hands-on session

Create a snakemake pipeline that utilizes the file `truth.csv` 
to evaluate the accuracy of the three classifiers `classifier_[123].py`. 

The file `truth.csv` contains a list of pairs
`region,classification` representing a genomic region and its
classification as `B (binign) / P (pathogenic)`.
Consider this file as the **ground truth** to compare the different classifiers.

Consider the following points, when writing the pipeline:

- Input/Output of each classifier. To make a comparison analysis it in necessary to convert formats
both for input and output.
- Each of these classifiers accept a paramater (seed) to perform their classification. Write a pipeline
that test the behaviour of the tools for this set of values `[0, 5, 10]`. 
Note that each method is using a different name for this argument
- Use the keyword `benchmark` provided by snakemake to evaluate execution times and memory usage of the tools.
- Produce you **final output** of the pipeline as a CSV file as follows:

```csv
tool,init,precision,recall,time,memory
classifier_1,0,60%,72%,2s,1MB
classifier_2,5,50%,90%,7s,8MB
...
```

**IMPORTANT**:
You must not change the code of the classifiers. It is not necessary to even look at them, 
consider them as black-box, you have to *deal* with them, but not modify them. 
Just as in real life :)


# Classifiers READMEs

### Classifier 1

Help:
```
usage: classifier_1.py [-h] --ref REF -i INPUT [--seed SEED] -o OUTPUT

Argument Parser Example

optional arguments:
  -h, --help            show this help message and exit
  --ref REF             Reference
  -i INPUT, --input INPUT
                        Input
  --seed SEED           An integer seed value
  -o OUTPUT, --output OUTPUT
                        Output file path
```

Input example:
```txt
chr10:1819916-1836627
chr16:2227361-2239388
chr22:6451132-6456071
```

Output example:
```txt
chr10:1819916-1836627,P
chr16:2227361-2239388,B
chr22:6451132-6456071,B
chr15:1711192-1733668,B
chr14:8037790-8046729,B

```

### Classifier 2

Help:
```
usage: classifier_2.py [-h] --fa FA --csvin CSVIN [--start START]

Argument Parser Example

optional arguments:
  -h, --help     show this help message and exit
  --fa FA        FASTA
  --csvin CSVIN  Input
  --start START  An integer start value
```

Input example:
```csv
chr10,1819916,1836627
chr16,2227361,2239388
chr22,6451132,6456071
chr15,1711192,1733668
chr14,8037790,8046729
```

Output example:
```csv
chr,start,end,classification
chr10,1819916,1836627,pathogenic
chr16,2227361,2239388,pathogenic
chr22,6451132,6456071,pathogenic
chr15,1711192,1733668,benign
chr14,8037790,8046729,pathogenic
```

### Classifier 3

Help:
```
usage: classifier_3.py [-h] -r REFFA --filein FILEIN [-s S] [--out OUT] [--ignore-cov]

Argument Parser Example

optional arguments:
  -h, --help            show this help message and exit
  -r REFFA, --reffa REFFA
                        FASTA
  --filein FILEIN       Input
  -s S                  An integer seed value
  --out OUT             Output file. Use - for stdout (default -).
  --ignore-cov          Ignore coverage value (default False).
```

Input example:
```csv
region,cov
chr10:1819916-1836627,0
chr16:2227361-2239388,0
chr22:6451132-6456071,0
chr15:1711192-1733668,0
```

Output example:
```csv
region,cov,prediction
chr10:1819916-1836627,0,p
chr16:2227361-2239388,0,p
chr22:6451132-6456071,0,p
chr15:1711192-1733668,0,b
```
