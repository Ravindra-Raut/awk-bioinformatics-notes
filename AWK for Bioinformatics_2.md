---
title: "AWK for Bioinformatics: FASTQ File Parsing"
author: "Ravindra Raut"
date: "`r format(Sys.Date(), '%B %d, %Y')`"
output:
  html_document:
    toc: true
    toc_float: true
    code_folding: show
---

# AWK for Bioinformatics: FASTQ File Parsing 🧬

This cheatsheet focuses on using **AWK** to efficiently manipulate, analyze, and convert data stored in the **FASTQ format**, a standard file format in sequencing.

---

## 1. 🔬 Understanding the FASTQ Structure

The key to processing FASTQ files is knowing that **every sequence record spans exactly four lines (records)**.

| Line Number | Content | AWK Condition (Line $N$ in record) |
| :--- | :--- | :--- |
| **Line 1** | Sequence Header (starts with `@`) | `NR % 4 == 1` |
| **Line 2** | **Sequence** (The DNA/RNA/Protein) | **`NR % 4 == 2`** |
| **Line 3** | Separator Line (starts with `+`) | `NR % 4 == 3` |
| **Line 4** | Quality Score | `NR % 4 == 0` |

---

## 2. 📊 Basic File Metrics and Preparation

These commands help you quickly assess the size and structure of your FASTQ file.

| Description | Command | Notes |
| :--- | :--- | :--- |
| **Count Total Lines (Linux)** | `cat SRR31346810_1.fastq | wc -l` | Standard Unix utility for counting lines. |
| **Count Sequences** | `awk 'END {print (NR/4)}' SRR31346810_1.fastq` | Divides total lines by 4 to get the number of reads. |
| **Create Small Sample** | `head -n 20 SRR31346810_1.fastq > seq.fastq` | Creates a smaller test file with 5 full reads (20 lines / 4). |
| **View with Line No.** | `awk '{print NR "\t" $0}' seq.fastq` | Helpful for visualizing the 4-line structure during debugging. |

**Example Code Chunk:**

\`\`\`{bash}
# Count the total number of reads (sequences) in the file
awk 'END {print "Total Sequences:", (NR/4)}' SRR31346810_1.fastq
\`\`\`

---

## 3. 🧪 Sequence Extraction and Analysis

Since the sequence is always on the **2nd line of the 4-line block**, we use the condition **`NR%4==2`**.

| Description | Command | AWK Function |
| :--- | :--- | :--- |
| **Print Sequences Only** | `awk 'NR%4==2 {print $0}' seq.fastq` | Filters and prints only the sequence lines. |
| **Sequence Length** | `awk 'NR%4==2 {print length($0)}' seq.fastq` | Uses the built-in `length()` function to report sequence length. |
| **Sequence and Length** | `awk 'NR%4==2 {print $0 "\t" length($0)}' seq.fastq` | Prints the sequence followed by its length. |
| **Calculate Average Length** | `awk 'BEGIN {sum=0}; NR%4==2 {sum=sum+length($0)}; END {print sum/(NR/4)}' seq.fastq` | Calculates total length and divides by the total sequence count. |

**Example Code Chunk:**

\`\`\`{bash}
# Extract sequence length for every read
awk 'NR%4==2 {print length($0)}' seq.fastq

# Calculate the mean sequence length
awk 'BEGIN {sum=0}; 
     NR%4==2 {sum+=length($0)}; 
     END {print "Average Length:", sum/(NR/4)}' seq.fastq
\`\`\`

---

## 4. 🔄 Format Conversion and Filtering

These commands demonstrate how to use AWK variables to process data across multiple lines (records), essential for file conversion.

### FASTQ to FASTA Conversion

```bash
# Convert FASTQ to FASTA format
awk '
NR%4==1 { a=$0; }                        # Line 1: Store the header (@line) in variable 'a'
NR%4==2 { print ">" a "\n" $0 }' seq.fastq > out_seq.fast
# Line 2: Print the FASTA header (>) using 'a', followed by the sequence ($0).