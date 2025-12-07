---
title: "AWK_Bioinformatics_Cheatsheet"
author: "Ravindra Raut"
output:
  html_document:
    toc: true
    toc_float: true
    code_folding: show
---

# AWK Command-Line Cheatsheet 📝

This document provides a quick reference guide to essential AWK commands for text processing and data manipulation, which is invaluable for working with tabular files in bioinformatics and data science.

---

## 1. 💻 Basic Syntax and Output

The fundamental structure of an AWK command is to specify a pattern and an action: `awk 'pattern {action}' filename`.

### Printing Fields and Lines

| Description | Command | Notes |
| :--- | :--- | :--- |
| **Print Entire Line** | `awk '{print $0}' marks.txt` | `$0` represents the whole record/line. |
| **Print Selected Fields** | `awk '{print $1"\t"$2"\t"$3}' marks.txt` | Prints columns 1, 2, and 3, separated by an explicit **tab** (`\t`). |
| **Field Separator** | `awk -F"\t" '{print $1"\t"$2"\t"$3}' marks.txt` | The `-F` flag sets the input field separator (here, to a tab). |
| **Print Last Field** | `awk '{print $NF}' marks.txt` | `NF` (Number of Fields) holds the index of the last column. |

**Example Code Chunk:**

\`\`\`{bash}
# Assuming 'marks.txt' is a tab-separated file:

# Print the 1st and 3rd columns
awk -F"\t" '{print $1"\t"$3}' marks.txt

# Print only the last column
awk '{print $NF}' marks.txt
\`\`\`

---

## 2. 🧮 Filtering and Conditionals

AWK uses conditional expressions (patterns) to control which lines are processed.

### Filtering by Value and Position

| Description | Command | Variable/Operator |
| :--- | :--- | :--- |
| **Conditional Operator** | `awk -F "\t" '$2=="M" {print $0}' marks.txt` | Prints lines where the 2nd field is exactly **"M"**. |
| **Print Line Number** | `awk -F "\t" '$2=="M" {print NR "\t" $0}' marks.txt` | `NR` (Number of Record) includes the line number (Sr No). |
| **Row Range** | `awk 'NR==2, NR==5 {print "\t" $0}' marks.txt` | Prints lines **from 2 up to and including 5**. |
| **Numeric Condition** | `awk '$4>80 {print "\t" $0}' marks.txt` | Prints lines where the 4th column value is **greater than 80**. |

### Searching and Regular Expressions

AWK uses the forward slash (`/pattern/`) for substring searching and regular expression matching.

| Description | Command | Regex Operator |
| :--- | :--- | :--- |
| **Search Anywhere** | `awk '/ANMOL/ {print "\t" $0}' marks.txt` | Prints lines containing **"ANMOL"** anywhere. |
| **Field Specific Search**| `awk -F "\t" '$3~/NEHA/ {print $0}' marks.txt` | Searches for **"NEHA"** only in the **3rd field**. |
| **Begins With** | `awk -F "\t" '$3~/^NEHA/ {print $0}' marks.txt` | The caret (`^`) anchors the search to the start of the field. |
| **Exact Field Match** | `awk -F "\t" '$3~/^NEHA$/ {print $0}' marks.txt` | Both `^` and `$` anchor the search to the start and end. |
| **Exact String Match** | `awk -F "\t" '$3=="NEHA" {print $0}' marks.txt` | Uses the equality operator (`==`) for strict string match. |
| **Search and Save** | `awk '/ANMOL/ {print "\t" $0}' marks.txt >>out22.txt` | Uses shell redirection (`>>`) to append output to a file. |

---

## 3. 🔢 Calculations and Control Blocks

The `BEGIN` and `END` blocks allow you to initialize variables and print summary statistics, respectively.

| Description | Command | Block Used |
| :--- | :--- | :--- |
| **Count Rows** | `awk 'BEGIN {count=0} {count++} END {print count}' marks.txt` | `BEGIN` initializes, the middle block increments, and `END` prints. |
| **Total Rows (Simple)** | `awk 'END {print NR}' marks.txt` | The simplest way to print the count of all records. |
| **Calculate Average** | `awk -F "\t" '{sum+=$4} END {print sum/(NR-1)}' marks.txt` | Accumulates sum in the middle block, calculates average in `END`. |
| **For Loop** | `awk 'BEGIN {sum=0; for (i=1; i<10; i++) sum=sum+i; print "Sum is " sum}'` | Executes a loop entirely within the `BEGIN` block (no input file needed). |

**Example Code Chunk:**

\`\`\`{bash}
# Calculate the average of values in the 4th column, excluding the header (NR-1)
awk -F "\t" '{sum+=$4} END {print "The average is: " sum/(NR-1)}' marks.txt
\`\`\`