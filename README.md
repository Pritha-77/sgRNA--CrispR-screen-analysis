Here's a copy-paste-ready `README.md` tailored to your **CRISPR screen analysis pipeline using R**. It covers the purpose, structure, software used, and step-by-step usage. You can save this as a file named `README.md` in your GitHub repo:

---

```markdown
# 🧬 CRISPR-Cas9 Screen Analysis Pipeline in R

This repository provides a complete pipeline for analyzing CRISPR-Cas9 pooled screen data using FASTQ files, sgRNA libraries, and differential abundance testing via DESeq2 in R.

---

## 📌 Purpose

The pipeline processes raw FASTQ reads, aligns them to a sgRNA reference (e.g., GeCKO library), and identifies genes significantly enriched or depleted in different experimental conditions (e.g., Control vs ToxA/ToxB).

---

## 🛠️ Features

- Read quality inspection with `ShortRead`
- sgRNA frequency analysis
- sgRNA reference generation from FASTA or Excel
- Alignment with `Rsubread`
- Differential analysis with `DESeq2`
- Output of enriched sgRNAs and summarized gene-level hits


---

## 🚀 Requirements

- R ≥ 4.1
- Internet connection (for downloading data/libraries)

### R Packages

```r
install.packages(c("ShortRead", "ggplot2", "dplyr", "BiocManager"))
BiocManager::install(c("Rsubread", "GenomicAlignments", "DESeq2", "Biostrings", "writexl"))
````

---

## 📂 Input Files

* **FASTQ files** from CRISPR screen experiments (paired-end)
* **sgRNA library** in `.tsv` or `.fastq` format (e.g., GeCKO)
* Optional: Pre-annotated sgRNA list from Addgene

---

## ▶️ Workflow Overview

### 1. 📥 Download & Load Data

* Set working directory:

  ```r
  setwd("D:/CRISPR")
  ```

* Download example dataset or use your own FASTQ files.

### 2. 🔍 FastQ Quality Checks

* Inspect base quality and base composition with `ShortRead` and `ggplot2`.

### 3. 🧬 sgRNA Reference Creation

* From FASTQ: convert to FASTA using `writeFasta()`
* From Excel/TSV: use `DNAStringSet()` from `Biostrings`

### 4. 📌 Indexing sgRNA Library

```r
buildindex("GeCKO", reference = "GeCKO.fa", indexSplit = FALSE)
```

### 5. 🎯 Alignment with Rsubread

* Align each FASTQ file to the sgRNA index
* Keep only exact matches (20 bp, 0 mismatches)

### 6. 📊 Count Extraction

* Use `table()` + `readGAlignments()` to extract sgRNA-level counts

### 7. 📈 DESeq2 Differential Analysis

```r
dds <- DESeqDataSetFromMatrix(myRes, colData = metadata, design = ~Group)
dds <- DESeq(dds)
```

* Compare: `ToxB vs Control`, `ToxA vs Control`

### 8. 🧪 sgRNA Enrichment

* Identify significantly enriched sgRNAs
* Merge with sgRNA-to-gene annotation
* Summarize at gene-level: mean log2FC, # enriched sgRNAs, etc.

### 9. 📤 Export Results

```r
write_xlsx(geneTable, "geneTable.xlsx")
```

