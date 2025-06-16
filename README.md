# Holstein Mammary Gland mRNA Timecourse

This repository contains the complete bioinformatic pipeline for analyzing transcriptional dynamics in the bovine mammary gland using mRNA sequencing data collected across five lactation stages.

## 📁 Project Overview

We profiled transcriptomic changes across five timepoints (Virgin [V], Mid-Pregnancy [MP], Late Pregnancy [LP], Early Lactation [EL], and Peak Lactation [PL]) in Holstein mammary epithelial tissue using a combination of raw FASTQ processing, normalization, and expression motif clustering.

## 📌 Sample Details

| Source          | Type         | Platform             | Timepoints Covered   |
|-----------------|--------------|----------------------|----------------------|
| Novogene        | SE (Unstr.)  | Illumina NovaSeq6000 | EL, LP, MP (468, 504, 509, 598, 610) |
| BCM             | PE (Str.)    | Illumina HiSeq2000   | V, LP (468, 502, 504, 509, 598, 610) |
| USDA-FAANG      | SE (Str.)    | Illumina NextSeq500  | EL, MP (502, 507)    |

⚠️ Mixed strandedness introduced batch effects, which were corrected downstream.

---

## 🧬 Pipeline Summary

### 1. Processing Raw Reads

**Alignment**
- Tool: `HISAT2 v2.2.1`
- Reference: `BosTau9`
- Mode: Single/paired-end, strand-specific

**Read Counting**
- Tool: `featureCounts v2.0.1`
- Parameters: Exon-level, uniquely mapped reads only

**Filtering**
- Retained genes expressed (counts ≥ 3) in ≥ 4 samples at any stage

**Batch Correction**
- Tool: `SVA`
- Function: `ComBatSeq()`
- Adjusted for library prep and strandedness using surrogate variables

**Normalization**
- Tool: `DESeq2`
- Method: Median-of-ratios, with size factor correction

---

### 2. Expression Motif Generation

- **Mean expression** calculated across replicates for each timepoint
- **Contrasts defined** between timepoints (e.g., MP vs V)
- **DESeq2 run** on raw normalized counts
- **Model vectors** constructed to track directional changes:
  - `"I"` = Increased
  - `"D"` = Decreased
  - `"S"` = No significant change (padj > 0.05)
- Each gene encoded as a 10-character vector summarizing transitions

---

### 3. Clustering of Expression Patterns

- Subset to significant genes (padj < 0.05 in any contrast)
- Averaged normalized counts across timepoints
- **Clustering tool**: `degPatterns()` (DIANA hierarchical clustering)
- Visualized using `degPlotCluster()`

---

## 📤 GEO Submission

Raw FASTQ and processed count matrices are available at:
**GEO Accession**: `GSE123456` *(placeholder)*

---

## 🧑‍🔬 Authors

- **Benji Lamp**, Ivan Ivanov, Monique Rijnkels, 
- With support from USDA-FAANG and AgResearch

---

## 💬 Contact

For questions:
**Benji Lamp** — `blamp25@tamu.edu`
