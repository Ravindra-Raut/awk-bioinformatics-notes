```


---
title: "R Notebook"
output: html_notebook
---

AWK programing: 
 
awk ‘ {action}’ filename

$0 for all fields:
awk '{print $0}' marks.txt

print column 1,2, 3 with tab separated:
awk '{print $1"\t"$2"\t"$3}' marks.txt

-F field separator:
awk -F"\t" '{print $1"\t"$2"\t"$3}' marks.txt

conditional operator $2=="M":
awk -F "\t" '$2=="M" {print $0}' marks.txt

NR for include the Sr No:
awk -F "\t" '$2=="M" {print NR "\t" $0}' marks.txt

last row NF:
awk '{print $NF}' marks.txt

only rows 2 to 5:
awk 'NR==2, NR==5 {print "\t" $0}' marks.txt

print on particular entry ANMOL:
awk '/ANMOL/ {print "\t" $0}' marks.txt

print on particular entry ANMOL and save in another file:
awk '/ANMOL/ {print "\t" $0}' marks.txt >>out22.txt

print on condition $4>80:
awk '$4>80 {print "\t" $0}' marks.txt

counting rows:
awk 'BEGIN {count=0} {count++} END {print count}' marks.txt

printing last rows:
awk 'END {print NR}' marks.txt

searching all NEHA:
awk '/NEHA/ {print "\t" $0}' marks.txt

searching all NEHA in 3rd row:
awk -F "\t" '$3~/NEHA/ {print $0}' marks.txt

searching all begins with NEHA:
awk -F "\t" '$3~/^NEHA/ {print $0}' marks.txt

searching all end with NEHA (begins with N, end with A):
awk -F "\t" '$3~/^NEHA$/ {print $0}' marks.txt

searching only NEHA:
awk -F "\t" '$3=="NEHA" {print $0}' marks.txt

calculate the average of column:
awk -F "\t" '{sum+=$4} END {print sum/(NR-1)}' marks.txt



for loop to calculate sum of 1 to 10:
awk 'BEGIN {sum=0; for (i=1; i<10; i++) sum=sum+i; print "Sum is " sum}'


Parsing a fastq file with awk
Count the words in linux:
cat SRR31346810_1.fastq | wc -l

Count the words:
awk 'END {print NR}' SRR31346810_1.fastq

Count the sequences:
awk 'END {print (NR/4)}' SRR31346810_1.fastq

make small data file with 5 lines, (20/4), 4 bcz of fastq:
head -n 20 SRR31346810_1.fastq >seq.fastq

see the file with row no, NR:
awk '{print NR "\t" $0}' seq.fastq

print only the sequences from fastq:
awk 'NR%4==2 {print $0}' seq.fastq

print the length of sequences from fastq:
awk 'NR%4==2  {print $0 "\t" length($0)}' seq.fastq

print the length of seq only:
awk 'NR%4==2  {print length($0)}' seq.fastq

convert fastq to fasta like:
awk 'NR%4==1{a=$0;} NR%4==2 {print ">" a "\t" $0}' seq.fastq >out_seq.fast

finding particular pattern in fastq, CAATGTG:
awk 'NR%4==1{a=$0} NR%4==2 && $0~/CAATGTG/ {print ">" a "\n" $0}' seq.fastq

calculate average length:
awk 'BEGIN {sum=0}; NR%4==2 {sum=sum+length($0)}; END {print sum/(NR/4)}' seq.fastq




```
