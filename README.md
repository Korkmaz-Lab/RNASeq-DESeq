# RNA-seq-DESeq-Workflow
A workflow for analysis of paired-end RNA-seq data and differential gene expression analysis.

## Overview

The pipeline uses **SLURM HPC** for alignment & quantification and **local R** for differential expression analysis and further visualization.

1. QC and Trimming with fastp on HPC.
2. Build STAR index with STAR on HPC.
3. Read alignment with STAR on HPC.
4. Build RSEM reference with RSEM on HPC.
5. Quantification with RSEM on HPC.
6. Differential Expression with DESeq2 on local R.

## Requirements

* **fastp** v1.0.1

    * GitHub Repo: https://github.com/OpenGene/fastp

    * Anaconda Download: https://anaconda.org/bioconda/fastp

* **STAR** v2.7.11b

    * GitHub Repo: https://github.com/alexdobin/STAR/tree/master

    * Anaconda Download: https://anaconda.org/bioconda/star

* **RSEM** v1.3.1

    * GitHub Repo: https://github.com/deweylab/RSEM
    * Installation Tutorial: https://github.com/bli25/RSEM_tutorial

* **DESeq2** (R)

    * https://bioconductor.org/packages/release/bioc/html/DESeq2.html

* **Assembly FASTA and annotation GTF files:** 

    * https://www.gencodegenes.org/human/

    * or download via wget: 
```
# The FASTA genome file (release 49 up-to-date as of Oct 2025)
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_49/GRCh38.primary_assembly.genome.fa.gz

# The GTF annotation file (release 49 up-to-date as of Oct 2025)
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_49/gencode.v49.annotation.gtf.gz
```

## Step 1: QC and Trimming with fastp

* **Input**

  * Raw read files (.fastq)

* **Output**

  * Trimmed files (trimmed.fq.gz)

Fastp processes the raw read files into trimmed.fq.gz files, which will be used in **step 3.** Additionally, an **html report** will be generated, which contains general information of the reads, before and after trimming statistics, and filtering results.

## Step 2: Build STAR index

A genome index is needed to be generated (only once) so that STAR can utilize it during the alignment/mapping, which is the next step. A reference genome sequence file (FASTA) and an annotation file (GTF) are required installed.

A star genome directory (~30 GB) will be generated, which contains necessary information of the genome and its annotations. It can be used for other analyses with the same genome reference and annotation.


## Step 3: Read Alignment with STAR
In this step, the trimmed read files are aligned to the reference genome generated in the previous step.

STAR genome directory, the annotation file, and the input directory containing the trimmed read files need to be specified below.


## Step 4: Build RSEM Reference
Similar to the STAR, RSEM also needs a reference index, which needs to be generated separately.

The exact assembly genome (FASTA) and annotation files (GTF) that were used during STAR index generation should be used.


## Step 5: Quantification with RSEM
The directory containing STAR output files, the generated RSEM reference directory, and RSEM bin directory containing the RSEM scripts (under software directory) should be specified. Sample names/types should also be specified.


RSEM will generate result files with (.genes.results) extension, which are needed for differential expression analysis in R with the code below.


## Step 6: Differential Expression Analysis with DESeq2 on local R

