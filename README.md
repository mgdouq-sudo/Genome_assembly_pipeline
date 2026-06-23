# Alteromonas macleodii Genome Assembly

## Overview

This repository contains a reproducible **hybrid genome assembly and analysis workflow** for *Alteromonas macleodii*, a marine bacterium. The project integrates **Nanopore long-read** and **Illumina short-read** sequencing data to generate a high-quality, polished genome assembly, followed by annotation and comprehensive quality assessment.

The workflow is implemented in **Nextflow** and executed on a high-performance computing environment using **Conda** to ensure reproducibility.

---

## Scientific Context

Hybrid genome assembly leverages the complementary strengths of long- and short-read sequencing technologies: long reads enable contiguous assemblies across repetitive regions, while short reads provide high base-level accuracy for polishing. This project applies a hybrid strategy to *A. macleodii* to generate a complete and well-annotated bacterial genome suitable for downstream comparative and functional analyses.

---

## Workflow Summary

### 1. Data Input

* Paired-end Illumina short reads and Nanopore long reads
* Sample paths provided in `bac_samples.csv`
* Reads organized into Nextflow channels:

  * `longread_ch`
  * `shortread_ch`

---

### 2. Quality Control

* **Short reads**: Quality assessment using **FastQC v0.12.1**
* **Long reads**: Filtering using **Filtlong v0.3.0** with parameters:

  * `--min_length 500`
  * `--keep_percent 90`

---

### 3. Genome Assembly & Polishing

* **De novo assembly** performed with **Flye v2.9.6** using filtered Nanopore reads
* **Short-read alignment**:

  * Index generation and alignment with **Bowtie2 v2.5.4**
  * Alignment processing with **SAMtools v1.22.1** (sorting and indexing)
* **Polishing**:

  * Assembly polishing using **Pilon v1.24** with Illumina alignments
  * Generates the final polished genome assembly

---

### 4. Genome Annotation

* Structural and functional annotation performed with **Prokka v1.14.6**
* Prediction of:

  * Protein-coding genes
  * rRNAs
  * tRNAs

---

### 5. Assembly Quality Assessment

* **Completeness** assessed using **BUSCO v6.0.0**

  * Lineage dataset: `alteromonas_odb12`
* **Assembly quality benchmarking** performed with **QUAST v5.3.0**

  * Comparison of polished and unpolished assemblies
  * Reference genome: *A. macleodii* strain ATCC 27126 (GCF_000172635.2)
  * Reference downloaded using **NCBI Datasets CLI v18.6.0**

---

### 6. Visualization

* Circular genome feature visualization generated using **PyCirclize** in a Jupyter Notebook
* Plots display genomic features such as CDS density and annotation tracks

---

## Execution Environment

* Workflow manager: **Nextflow v25.04.6**
* Execution platform: **Boston University Shared Computing Cluster (SCC)**
* Environment management: **Conda**

All tools were executed with default parameters unless otherwise specified.

---

## Repository Contents

* Nextflow pipeline scripts for hybrid genome assembly
* `bac_samples.csv` – sample input file with read paths
* Configuration files for Conda environments and cluster execution
* Jupyter Notebook for genome visualization with PyCirclize

---

## Outputs

* Draft and polished genome assemblies (FASTA)
* Annotated genome files (GFF, GBK)
* Quality control and benchmarking reports:

  * FastQC
  * BUSCO
  * QUAST
* Circular genome visualization plots

---

## Reproducibility

This pipeline is fully reproducible through the use of Nextflow workflows, explicit tool versioning, and Conda-managed software environments. Running the workflow with the same inputs will yield identical results across systems.

---

## Author

Mohammad Gharandouq

---

## Reference Genome

*A. macleodii* strain ATCC 27126 — RefSeq accession **GCF_000172635.2**
