# Blastp-using-Linux
Steps to do Blastp search on the genomes 


Here is a README file for your GitHub repository that includes the code and a detailed explanation of each line:

---

# BLAST Search Pipeline for Protein Sequences

This repository contains a simple pipeline for performing a BLASTP search of protein sequences against a custom protein database, filtering the results by e-value, and extracting the matching sequences.

## Files

- `wheat_cv.faa`: Protein sequences of wheat cultivar (input database)
- `yield.fasta`: Query protein sequences to search against the database
- `blast_results.tsv`: Raw BLASTP output in tabular format
- `filtered_results.tsv`: BLASTP results filtered by e-value threshold
- `yield_blast_sequences.fasta`: Extracted sequences from the database matching the filtered BLAST hits

## Pipeline Steps and Explanation

### Step 1: Create a BLAST protein database

```bash
makeblastdb -in wheat_cv.faa -dbtype prot -out genome_db
```

- `makeblastdb`: Command to create a BLAST database.
- `-in wheat_cv.faa`: Input file containing protein sequences in FASTA format.
- `-dbtype prot`: Specifies that the database type is protein.
- `-out genome_db`: Output prefix for the created BLAST database files.

This step prepares the wheat protein sequences as a searchable BLAST database named `genome_db`.

---

### Step 2: Run BLASTP search

```bash
blastp -query /yield.fasta -db genome_db -out blast_results.tsv -outfmt 6
```

- `blastp`: BLAST program to compare protein query sequences against a protein database.
- `-query yield.fasta`: Input query protein sequences in FASTA format.
- `-db genome_db`: The BLAST database created in Step 1.
- `-out blast_results.tsv`: Output file to save BLAST results.
- `-outfmt 6`: Output format 6 is tabular with standard columns (e.g., query id, subject id, % identity, e-value).

This step searches the query proteins against the wheat protein database and saves the results in a tabular format.

---

### Step 3: Filter BLAST results by e-value

```bash
awk '$11 <= 1e-10' blast_results.tsv > filtered_results.tsv
```

- `awk`: A text processing tool.
- `'$11  filtered_results.tsv`: Redirects the filtered output to a new file.

This step keeps only highly significant BLAST hits with e-values ≤ 1e-10.

---

### Step 4: Extract matching sequences from the database

```bash
seqtk subseq wheat_cv.faa <(awk '{print $2}' filtered_results.tsv) > yield_blast_sequences.fasta 
```

- `seqtk subseq`: A tool to extract subsequences from a FASTA file.
- `wheat_cv.faa`: The original protein FASTA file (database sequences).
- ` yield_blast_sequences.fasta`: Redirects the extracted sequences to a new FASTA file.

This step extracts the protein sequences from the wheat database that matched the query sequences with significant BLAST hits.

---

## Requirements

- BLAST+ suite installed (`makeblastdb`, `blastp`)
- `awk` (usually pre-installed on Unix/Linux systems)
- `seqtk` installed for sequence extraction

## Usage

Run each step sequentially in a Unix/Linux terminal to perform the BLAST search, filter results, and extract sequences.

---




