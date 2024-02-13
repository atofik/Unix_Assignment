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

## Data processing
```
grep -E "(Group|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt | cut --complement -f 2-3 > temp_maize_genotypes.txt
awk -f transpose.awk temp_maize_genotypes.txt > temp_transposed_maize_genotypes.txt
```

1. the grep command here is extracting the maize genotype and redirecting it to a new file. Then the awk command uses awk to transpose.awk on the file tem_maize_genotypes.txt and then redirects the output to a new file 

```
cut -f 1,3,4 snp_position.txt > temp_positions.txt
head  temp_positions.txt | column -t 
```
2. This command uses the cut utility in Unix to extract specific fields from a file named snp_position.txt and then redirects the output to a new file called temp_positions.txt

 ```
$ grep -v "^SNP_ID" temp_positions.txt | sort -k1,1 > temp_sorted_positions.txt
$ head temp_sorted_positions.txt
 ```
3. This command sequence involves the use of grep, sort, and redirection to process and organize data from a file named temp_positions

```
$ grep -v "^Sample" temp_transposed_maize_genotypes.txt | sort -k1,1 > temp_sorted_maize_genotypes.txt
```
4. This command removes lines from temp_transposed_maize_genotypes.txt that start with "Sample", sorts the remaining lines based on the first field, and saves the sorted data to a new file called temp_sorted_maize_genotypes

```
head -n 1 temp_transposed_maize_genotypes.txt > temp_maize_header.txt
head -n 1 temp_positions.txt > temp_position_header.txt

```
5. Here head is used to extract the first line from the file temp_transposed_maize_genotypes.txt and redirects (>) that line into a new file called temp_maize_header.txt. Essentially, it's taking the header (which is usually the first line in a file containing column names or some sort of titles) from a transposed genotype data file and saving it separately. 
```
awk -F "\t" '{print NF; exit}' temp_sorted_maize_genotypes.txt
awk -F "\t" '{print NF; exit}' temp_maize_genotypes.txt
cat tem_maize_header.txt temp_sorted_maize_genotypes.txt > temp_sorted_maize_complete.txt

```
5. here we checked to make sure that the number of columns (1574) is the same for the two files then cat command combines two files, tem_maize_header and tem_sorted_maize_genotypes, into a single file named tem_sorted_maize_complete

```
awk -F "\t" '{print NF; exit}' temp_position_header.txt
awk -F "\t" '{print NF; exit}' temp_sorted_positions.txt
cat temp_position_header.txt temp_sorted_positions.txt > temp_sorted_positions_complete.txt

```
6. the awk command was used to check the number of columsn (3) which was same for both then the cat combines the two files toger

```
$ awk -F "\t" '{print NF; exit}' temp_sorted_positions_complete.txt
$ awk -F "\t" '{print NF; exit}' temp_sorted_maize_complete.txt
$ join -1 1 -2 1 -t $'\t' --header temp_sorted_positions_complete.txt temp_sorted_maize_complete.txt > temp_merged_maize.txt
```
7. here i inspected the number columns in both files then by using join command it merge lines of two files based on a common field.

```
grep -E '(Chromosome|unknown)' temp_merged_maize.txt | head -n 2
grep -E '(Chromosome|unknown)' temp_merged_maize.txt > maize_unknown_pos_snps.txt
```
8. I used the commands here to inspect the content of the file to see unknown chromosome then exrated the files with unknown chromosme into a new file

```
grep -E '(Chromosome|multiple)' temp_merged_maize.txt > maize_multiple_pos_snps.txt
```
9. here i exrated the files with multiple chromosme into a new file

```
grep -v -E "(unknown|multiple)" temp_merged_maize.txt > temp_merged_maize_chromosomes.txt
```
10. This command filters lines from the file temp_merged_maize.txt to exclude those containing the words "unknown" or "multiple", saving the result to a new file

```
head -n 1 temp_merged_maize_chromosomes.txt > temp_merged_maize_headers.txt
```
11. The command here extracts the first line (header) from the file temp_merged_maize_chromosomes.txt and saves it to a new file named temp_merged_maize_headers

```
sed 's/\?\/\?/\?/g' temp_merged_maize_chromosomes.txt | sort -k2,2n -k3,3n | awk -F '\t' '{print $0 > "temp_maize_inc_c_"$2.txt}'
```
12. This sed command here isused for subtituting standardizes unknown data (?/?) missing data values and replaces it with single '?', then sorts the full dataset. Then the sort command sorts the output from the sed command. The sorting is done first on the second column (-k2,2n) in numerical order (due to n), and then on the third column (-k3,3n) in increasing order

```
for i in $(seq 1 10); do cat temp_merged_maize_headers.txt temp_m_inc_c_$i.txt > "maize_inc_c_${i}.txt"; done
```
13. The code loops through numbers 1 to 10, concatenating a header file with each of a series of data files named temp_maize_inc_c_1 to temp_maize_inc_c_10. It then saves each concatenated result into a new file with each file named maize_inc_c_1.txt through maize_inc_c_10.txt

```
sed 's/?\/?/-/g' temp_merged_maize_chromosomes.txt | sort -k2,2n -k3,3nr | awk -F '\t' '{print $0 > "temp_maize_dec_c_"$2}'
```
14. Just like the previous command, here and replace all occurrences of ?/? with '-'. This substitution is applied globally within each line of the input file then sorts it in a decreasing order specified by the -k3,3nr part of the sort command.

```
for i in $(seq 1 10); do cat tem_merged_maize_headers.txt tem_m_dec_c_$i.txt > "maize_dec_c_"$i".txt"; done
```
15. This code snippet is a bash script loop that concatenates the header row from a file named tem_merged_maize_headers with ten different files named tem_m_dec_c_$i, where $i represents numbers from 1 through 10. Each iteration of the loop concatenates the header row with one of these ten files and outputs the result into a new file in a directory. The new files are named maize_dec_c_$i.txt, again with $i representing the sequence number from 1 to 10

rm tem*
