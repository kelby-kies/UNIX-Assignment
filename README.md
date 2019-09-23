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
 
 ### Maize Data
 ```
 here is my snippet of code used for data processing
 ```
 here is my brief description of what this code does
 
 ### Teosinte Data
 ```
 here is my snippet of code used for data processing
 ```
 Here is my brief description of what this code does
