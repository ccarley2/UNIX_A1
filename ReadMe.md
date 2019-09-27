

# UNIX Assignment Clayton Carley

## Data Inspection

### Attributes of `fang_et_al_genotypes`


Here is my snippet of code used for data inspection:
```
$ head fang_et_al_genotypes.txt
$ wc -l fang_et_al_genotypes.txt
$ du -h fang_et_al_genotypes.txt
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```

By inspecting this file I learned that:
* From the basic head I was able to see that the data is created in rows as opposed to conventional columns. The data looks like a lot of SNP names and the peceding data would be the affiliated SNP calls with a lot of missing call data represented by "?"
* There are 2783 lines in the raw data of the file. 
* The file is 6.1M in size.
* Initial inspection shows 986 columns, but this is due to the row set up of the file. 
* When transposed the file has 2783 columns. 

### Attributes of `snp_position.txt`


Here is my snippet of code used for data inspection:
```
$ head snp_position.txt
$ awk -F "\t" '{print NF; exit}' snp_position.txt
$ wc -l snp_position.txt
$ du -h snp_position.txt

```
By inspecting this file I learned that:
* From the head and column inspection I can see that there are 15 columns with line one being the header information. 
* There are 984 lines which would suggest 983 SNPs represented in the file. 
* The file is 38K in size. 

## Data Processing
### Pre-processing
```
$ grep ZM[MP] fang_et_al_genotypes.txt > fang_ZM
$ head fang_et_al_genotypes.txt -n1 > fang_head
$ cat fang_ZM >> fang_head
$ cat fang_head > ZM
```
created a file of just maize and teosinte data then added the head back to it and renamed it ZM. 



### Maize Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does


### Teosinte Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does

> Written with [StackEdit](https://stackedit.io/).
