# Unix_Assignment
Unix Assignment 1
## Data inspeciton
### Attributes of 'fang_et_al_genotypes'
```
here is my snippet of code used for data inspection
```
```
1. $ du -h fang_et_al_genotypes.txt snp_positon.txt
#file size 6.7 for fang and 49kb for snp

2. $ file fang_et_al_genotypes.txt snp_position.txt
#tells us the file type and both files are ASCII text and ASCII text with very long lines for  fang_et_al_genotypes.txt

3. $ wc snp_position.txt fang_et_al_genotypes.txt
#used to knonw the number lines, words, characters
     
2. $ wc -l fang_et_al_genotypes.txt snp_position.txt
#numbmer of line for fang was 2783 and snp was 984

3. $ head fang_et_al_genotypes.txt snp_position.txt
 #looked at to find if there were headers if available no hashs but line one is header 



4. $ grep -v "^#" fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
#number of columns fof fang was 986 and 

5. $ grep -v "^#" snp_position.txt | awk -F "\t" '{print NF; exit}' snp_position.txt
#number of columns for snp was 15 columns

6. $ tail -n +2 snp_position.txt | cut -f1-2 | column -t
#since line was a title this command was used to see if the file was sorted or not after extracting column 1 and 2

```
by inspecting the file i learned that 
1. fang_et_al_genotypes.txt is 6.7MB and snp_position.txt is 49kb
2. fang_et_al_genotypes.txt has 2783 lines while  snp_position.txt has 984 lines
3. 
