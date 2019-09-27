# UNIX-Assignment

## Data Inspection

### Attributes of fang_et_al_genotypes
```
du -h
tail -n +2 fang_et_al_genotypes.txt |awk -F "\t" '{print NF; exit}'
wc -l
wc -w
wc -c
```
 By inspecting this file I learned that:
 
 * File Size: 11M
 * Number of columns: 986
 * Number of lines: 2783
 * Number of words: 2,744,038
 * Number of bytes: 11,051,939
 
 ### Attributes of 'snp_position.txt'
 
 ```
du -h
awk -F "\t" '{print NF; exit}' snp_position.txt
wc -l
wc -w
wc -c
 ```
 By inspecting this file I learned that:

 * File Size: 84K
 * Number of columns: 15
 * Number of lines: 984
 * Number of words: 13,198
 * Number of bytes: 82,763

 ## Data Processing

Saved fang_et_al_genotypes.txt header:
```
head -n 1 fang_et_al_genotypes.txt > fang_header.txt
```

Seperate the maize and teosinte data out of fang_et_al_genotypes.txt:
```
grep -E "ZMMIL|ZMMLR|ZMMMR" fang_et_al_genotypes.txt > maize.txt
grep -E "ZMPBA|ZMPIL|ZMPJA" fang_et_al_genotypes.txt > teosinte.txt

``` 

 ### Maize Data
Add the fang_header.txt and maize.txt:

 ```
cat fang_header.txt maize.txt > maize_with_header.txt 
 ```
 
Get rid of excess metadata:
```
cut -f 3-986 maize_with_header.txt > maize_with_header_pretranspose.txt
```

Transpose data:
```
awk -f transpose.awk maize_with_header_pretranspose.txt > transposed_maize.txt
```

Remove Header:
```
tail –n +2 transposed_maize.txt > transposed_maize_nohead.txt
```

Sort by the first column:
```
sort -k1 transposed_maize_nohead.txt > sort_transposed_maize.txt
```

Join the maize files and snp_files:
```
join -1 1 -2 1 -t $'\t' sort_snp_position.txt sort_transposed_maize.txt > maize_snp_join.txt
```

Filter out the multiple position & unknown position files:
```
grep -Ev multiple maize_snp_join.txt | grep -Ev unknown maize_snp_join.txt > known_maize_snp.txt
```

Save the multiple/unknown files:
```
grep -E multiple maize_snp_join.txt > multiple_positions_maize.txt
grep -E unknown maize_snp_join.txt > unknown_positions_maize.txt
```

Save 1-10 chromosomes in specified way:
Increasing and encoded with ?: (Repeat for 1-10)
```
awk '$3 == 1' known_maize_snp.txt | sort -k3,3n > maize_chrom1_increasing.txt
```

Decreasing and encoded with -: (Repeat for 1-10)
```
awk '$3==1' known_maize_snp.txt | sort -k3,3n -r > maize_chrom1_decreasing.txt
sed 's/?/-/g' maize_chrom1_decreasing.txt > maize_chrom1_decr_encoded-.txt
```

Cut the rest of metadata:
```
cut -f 1,3,4,16-1588 maize_chrom1_increasing.txt > maize_chrom1_inc.txt
cut -f 1,3,4,16-1588 maize_chrom1_decr_encoded-.txt > maize_chrom1_decr.txt
cut -f 1,3,4,16-1588 multiple_positions_maize.txt > maize_multiple.txt
cut -f 1,3,4,16-1588 unknown_positions_maize.txt > maize_unknown.txt
```

Append a final header to each file:
```
cat final_header.txt maize_unknown.txt > maize_unknown_final.txt
cat final_header.txt maize_chrom1_incr.txt > maize_chrom1_incr_final.txt
cat final_header.txt maize_chrom1_decr.txt > maize_chrom1_decr_final.txt
```
 
 ### Teosinte Data
Add the fang_header.txt and teosinte.txt:

 ```
cat fang_header.txt teosinte.txt > teosinte_with_header.txt 
 ```
 
Get rid of excess metadata:
```
cut -f 3-986 teosinte_with_header.txt > teosinte_with_header_pretranspose.txt
```

Transpose data:
```
awk -f transpose.awk teosinte_with_header_pretranspose.txt > transposed_teosinte.txt
```

Remove Header:
```
tail –n +2 transposed_teosinte.txt > transposed_teosinte_nohead.txt
```

Sort by the first column:
```
sort -k1 transposed_teosinte_nohead.txt > sort_transposed_teosinte.txt
```

Join the teosinte files and snp_files:
```
join -1 1 -2 1 -t $'\t' sort_snp_position.txt sort_transposed_teosinte.txt > teosinte_snp_join.txt
```

Filter out the multiple position & unknown position files:
```
grep -Ev multiple teosinte_snp_join.txt | grep -Ev unknown teosinte_snp_join.txt > known_teosinte_snp.txt
```

Save the multiple/unknown files:
```
grep -E multiple teosinte_snp_join.txt > multiple_positions_teosinte.txt
grep -E unknown teosinte_snp_join.txt > unknown_positions_teosinte.txt
```

Save 1-10 chromosomes in specified way:
Increasing and encoded with ?: (Repeat for 1-10)
```
awk '$3 == 1' known_teosinte_snp.txt | sort -k3,3n > teosinte_chrom1_increasing.txt
```

Decreasing and encoded with -: (Repeat for 1-10)
```
awk '$3==1' known_teosinte_snp.txt | sort -k3,3n -r > teosinte_chrom1_decreasing.txt
sed 's/?/-/g' teosinte_chrom1_decreasing.txt > teosinte_chrom1_decr_encoded-.txt
```

Cut the rest of metadata:
```
cut -f 1,3,4,16-1588 teosinte_chrom1_increasing.txt > teosinte_chrom1_inc.txt
cut -f 1,3,4,16-1588 teosinte_chrom1_decr_encoded-.txt > teosinte_chrom1_decr.txt
cut -f 1,3,4,16-1588 multiple_positions_teosinte.txt > teosinte_multiple.txt
cut -f 1,3,4,16-1588 unknown_positions_teosinte.txt > teosinte_unknown.txt
```

Append a final header to each file:
```
cat final_header.txt teosinte_unknown.txt > teosinte_unknown_final.txt
cat final_header.txt teosinte_chrom1_incr.txt > teosinte_chrom1_incr_final.txt
cat final_header.txt teosinte_chrom1_decr.txt > teosinte_chrom1_decr_final.txt
```
