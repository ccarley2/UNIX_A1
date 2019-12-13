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

First I stripped the head from the Fang file and created new text files for the maize and teosinte data.
```
$ head -n 1 fang_et_al_genotypes.txt > maize.txt
$ head -n 1 fang_et_al_genotypes.txt > teosinte.txt
```
Next I extracted the maize data of ZMMIL ZMMLR and ZMMMR and the teosinte data of ZMPBA ZMPIL ZMPJA from the Fang file and added them to their respective .txt files using grep. 
```
$ grep -e "ZMMIL" -e "ZMMLR" -e "ZMMMR"  fang_et_al_genotypes.txt >> maize.txt

$ grep  -e "ZMPBA" -e "ZMPIL" -e "ZMPJA" fang_et_al_genotypes.txt >> teosinte.txt
```
Next I trasnposed the new files using the awk function. 
```
$ awk -f transpose.awk maize.txt > maize_tran.txt
$ awk -f transpose.awk teosinte.txt > teosinte_tran.txt
```

### Maize Data

This is how I extracted the headders and used VI to manually change the new header for the fang file to have the nessecary SNP_ID, Chromosome, and Position columns.  I then pulled the needed columns and pasted them together.  
```
$ tail -n +4 maize_tran.txt > maize_tran_short.txt
$ head -n 1 maize_tran.txt > maize_header.txt
$ vi maize_header.txt
$ cut -f 1 snp_position.txt > short_snp.txt
$ cut -f 3 snp_position.txt > short_snp_chrom.txt
$ cut -f 4 snp_position.txt > short_snp_pos.txt
```
Next I sort the new files to goin the SNP info with the genotypes making sure that the SNP_IDs are both in the same order. 
```
$ sort -k1,1 short_snp_all.txt > short_snp_all_sort.txt 
$ sort -k1,1 maize_tran_short.txt > maize_tran_short_sort.txt
```
Next I joined the files together and appended the header file. 
```
$ join -1 1 -2 1 -t $'\t' -e 'empty' short_snp_all_sort.txt maize_tran_short.txt > maize_join_full.txt	
$ cat maize_header.txt maize_join_full.txt > MAIZE.txt
```
I made a file to check everything was happening right. 
```
$ cat maize_header.txt maize_chrom_01_IN_headless.txt > maize_chrom_01_increase_W_head.txt
```
Next I created the requested maize file for each chromosome with the SNP order in increasing positions and placed it in the Maize directory as an input file. I just changed the chromosome search and name for each chromosome.  
```
$ sort -k2,2n -k3,3n MAIZE.txt > MAIZE_sort.txt 
$ awk '{ if ($2 == 1 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_01_incr.txt
$ awk '{ if ($2 == 2 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_02_incr.txt
$ awk '{ if ($2 == 3 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_03_incr.txt
$ awk '{ if ($2 == 4 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_04_incr.txt
$ awk '{ if ($2 == 5 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_05_incr.txt
$ awk '{ if ($2 == 6 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_06_incr.txt
$ awk '{ if ($2 == 7 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_07_incr.txt
$ awk '{ if ($2 == 8 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_08_incr.txt
$ awk '{ if ($2 == 9 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_09_incr.txt
$ awk '{ if ($2 == 10 && $3 !~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_10_incr.txt
```
Next I made the ten files with snips in reverse order and the ? replaced with -
```
$ sort -k2,2n -k3,3nr MAIZE.txt > MAIZE_reverse.txt
$ sed 's/?/-/g' MAIZE_reverse.txt > MAIZE_reverse_sort.txt
$ awk '{ if ($2 == 1 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_01_decr.txt
$ awk '{ if ($2 == 2 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_02_decr.txt
$ awk '{ if ($2 == 3 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_03_decr.txt
$ awk '{ if ($2 == 4 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_04_decr.txt
$ awk '{ if ($2 == 5 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_05_decr.txt
$ awk '{ if ($2 == 6 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_06_decr.txt
$ awk '{ if ($2 == 7 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_07_decr.txt
$ awk '{ if ($2 == 8 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_08_decr.txt
$ awk '{ if ($2 == 9 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_09_decr.txt
$ awk '{ if ($2 == 10 && $3 !~ /multiple/) { print } }' MAIZE_reverse_sort.txt > Maize/maize_chrom_10_decr.txt
```
Then I made 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)
```
$ awk '{ if ($2 ~ /unknown/ || $3 ~ /unknown/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_unknown.txt
```
Followed by 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
```
$ awk '{ if ($2 ~ /multiple/ || $3 ~ /multiple/) { print } }' MAIZE_sort.txt > Maize/maize_chrom_multiple.txt
```
### Teosinte Data

This is how I extracted the headders and used VI to manually change the new header for the fang file to have the nessecary SNP_ID, Chromosome, and Position columns.  I then pulled the needed columns and pasted them together.  
```
$ tail -n +4 teosinte_tran.txt > teosinte_tran_short.txt
$ head -n 1 teosinte_tran.txt > teosinte_header.txt
$ vi teosinte_header.txt
$ cut -f 1 snp_position.txt > short_snp.txt
$ cut -f 3 snp_position.txt > short_snp_chrom.txt
$ cut -f 4 snp_position.txt > short_snp_pos.txt
```
Next I sort the new files to goin the SNP info with the genotypes making sure that the SNP_IDs are both in the same order. 
```
$ sort -k1,1 short_snp_all.txt > short_snp_all_sort.txt 
$ sort -k1,1 teosinte_tran_short.txt > teosinte_tran_short_sort.txt
```
Next I joined the files together and appended the header file. 
```
$ join -1 1 -2 1 -t $'\t' -e 'empty' short_snp_all_sort.txt teosinte_tran_short.txt > teosinte_join_full.txt	
$ cat teosinte_header.txt teosinte_join_full.txt > TEOSINTE.txt
```
Next I created the requested maize file for each chromosome with the SNP order in increasing positions and placed it in the Teosinte directory as an input file. I just changed the chromosome search and name for each chromosome.  
```
$ sort -k2,2n -k3,3n TEOSINTE.txt > TEOSINTE_sort.txt

$ awk '{ if ($2 == 1 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_01_incr.txt
$ awk '{ if ($2 == 2 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_02_incr.txt
$ awk '{ if ($2 == 3 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_03_incr.txt
$ awk '{ if ($2 == 4 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_04_incr.txt
$ awk '{ if ($2 == 5 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_05_incr.txt
$ awk '{ if ($2 == 6 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_06_incr.txt
$ awk '{ if ($2 == 7 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_07_incr.txt
$ awk '{ if ($2 == 8 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_08_incr.txt
$ awk '{ if ($2 == 9 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_09_incr.txt
$ awk '{ if ($2 == 10 && $3 !~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_10_incr.txt
```

Next I made the ten files with snips in reverse order and the ? replaced with -

```
$ sort -k2,2n -k3,3nr TEOSINTE.txt > TEOSINTE_reverse.txt

$ sed 's/?/-/g' TEOSINTE_reverse.txt > TEOSINTE_reverse_sort.txt

$ awk '{ if ($2 == 1 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_01_decr.txt
$ awk '{ if ($2 == 2 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_02_decr.txt
$ awk '{ if ($2 == 3 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_03_decr.txt
$ awk '{ if ($2 == 4 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_04_decr.txt
$ awk '{ if ($2 == 5 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_05_decr.txt
$ awk '{ if ($2 == 6 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_06_decr.txt
$ awk '{ if ($2 == 7 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_07_decr.txt
$ awk '{ if ($2 == 8 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_08_decr.txt
$ awk '{ if ($2 == 9 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_09_decr.txt
$ awk '{ if ($2 == 10 && $3 !~ /multiple/) { print } }' TEOSINTE_reverse_sort.txt > Teosinte/teosinte_chrom_10_decr.txt
```

Then I made 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)

```
$ awk '{ if ($2 ~ /unknown/ || $3 ~ /unknown/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_unknown.txt
```
Followed by 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
```
$ awk '{ if ($2 ~ /multiple/ || $3 ~ /multiple/) { print } }' TEOSINTE_sort.txt > Teosinte/teosinte_chrom_multiple.txt
```
> Written with [StackEdit](https://stackedit.io/).
