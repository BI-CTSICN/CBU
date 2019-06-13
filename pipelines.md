---
layout: page
title: Pipelines
menu: header
---

## **RNA Seq**

FastQ files containing the raw RNA-Seq reads (Bulk RNA-seq)  output from the sequencers are first checked for quality using FastQC. 

It is followed by trimming of poor quality reads using any of the following tools

* Trimmomatic 
* BBDuk
* cutadapt

An aligner is used to align the reads to the reference genome (Human/Mouse etc) using any of the following tools
*	Tophat2
*	Hisat2
*	STAR - Recommended by the analysts based on reports by different groups as well as the following review https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5792058/

The aligned Sequence Alignment Map (SAM) files are sorted and converted to BAM using samtools in some cases

Next, the Binary Alignment Map (BAM) files are checked for their quality using either of the following tools
*	Picard RNASeqmetrics
*	RSeQC

In cases for fusion gene detection, we specifically use either the Tophat-fusion or STAR-Fusion tool. While splicing events are identified using SGSe tool part of the biocondutor package in R.

Quantification of reads to Transcripts per million (TPM) is calculated using RSEM.

For cases of high sequence duplication, we tag the duplicates using Picard MarkDuplicates and count the duplicated reads using featureCounts. In many cases of overduplication we also remove the duplicated reads using Picard MarkDuplicates.

For raw unnormalized counts, we use either of the following tools:
*	Htseq Counts
*	featureCounts


For Differential Gene expression, we use either the following packages in R:
*	Deseq2
*	EdgeR
*	Ebseq2

The final data can be represented in the form of heatmaps, volcano plots, MA plots and PCA plots using basic R packages. 

### Requirements from the Investigator for the Analysis

a.	Fastq files (Required)

b.	Organism used (Required)

c.	Platform used for the sequencing (Optional)

d.	Sample sheet (Required). Sample sheet should have the following:

I)	Sample names (required)

II)	Conditions associated to each samples (required)

III)	Lane information (required)

IV)	Sample concentration (optional for bulk; required for Single cell)

V)	Spike in controls used (optional)

VI)	Number of cells used per sample (required for Single cell)

VII)	Quality of RNA used (optional)

e.	Candidate gene list (optional)

For **single cell RNA seq analysis** for 10x genomics we use the cellranger pipeline and Seurat for differential expression. For other pipelines we use demultiplexing algorithms (as recommended by the kit manufacturers) and follow alignment and counting pipelines as described for normal RNA sequencing. Differential expression estimated using Seurat/Monocle. 

## **WGS/Exome Seq**

The raw sequencing data in fastqc files is first quality checked with FastQC and depending upon the quality it is trimmed using the following tools.
* Trimmomatic 
* BBDuk
* cutadapt

An aligner is used to align the reads to the reference genome (Human/Mouse etc) using any of the following tools
*	Bwa-mem
*	Bowtie2

The aligned Sequence Alignment Map (SAM) files are processed  to Binary Alignment Map (BAM) files, 
*	samtools  
*	Picard tools

We follow the Genome Analysis Toolkit 4 (GATK 4),  pipeline to do Variant Calling. The BAM files are further processed for using Base Quality Score Recalibration (BQSR) step to detect and correct base calling errors from sequencers:
*	BaseRecalibrator
*	ApplyBQSR

Variant calling is performed on the aligned BAM files, producing genome variant call format (gvcf) files using HaplotypeCaller function.

The gvcfs are combined using CombineGVCFs function, followed by running the GenotypeGVCFs function, to increase the sensitivity of the variants called. Output of this step is a vcf file,

The vcf file is then further processed  using Variant Quality Score Recalibration (VQSR) tools to increase the quality of variants identified, i.e. decrease the number of false positives. 

*	VariantRecalibrator 
*	ApplyVQSR 

Annotation of variant is done using Annovar, followed by manual filtration.

### Requirements from the Investigator for the Analysis

a.	Fastq files (Required)

b.	Organism used (Required)

c.	Platform used for the sequencing (Optional)

d.	Sample sheet (Required). Sample sheet should have the following:
VIII)	Sample names (required)

IX)	If family trio being run relationship should be mentioned (required)

X)	Lane information (required)


## **T-cell Receptor Sequencing**

The raw sequencing data in fastqc files is first quality checked with FastQC and depending upon the quality it is trimmed using the following tools.
* Trimmomatic 
* BBDuk
* cutadapt

Extraction and alignment of fragments of target molecules is performed followed by assembly of overlapping fragmented sequencing reads into long-enough CDR3 containing contigs using the MiXCR pipeline to analyze TCR or Ig repertoire from sequencing data.

### Requirements from the Investigator for the Analysis

a.	Fastq files (Required)

b.	Organism used (Required)

c.	Platform used for the sequencing (Optional)

d.	Sample sheet (Required). Sample sheet should have the following:

I)	Sample names (required)

II)	Conditions associated to each samples (required)

III)	Lane information (required)

IV)	Sample concentration (optional for bulk; required for Single cell)


## **Metagenomics/Microbiome**

The raw sequencing data in fastqc files is first quality checked with FastQC and depending upon the quality it is trimmed using the following tools.
* Trimmomatic 
* BBDuk
* cutadapt

We perform demultiplexing and quality filtering, OTU picking, taxonomic assignment, and phylogenetic reconstruction, and diversity analyses and visualizations using QIIME1 pipeline.

### Requirements from the Investigator for the Analysis

a.	Fastq files (Required)

b.	Organism used (Required)

c.	Platform used for the sequencing (Optional)

d.	Sample sheet (Required). Sample sheet should have the following:

I)	Sample names (required)

II)	Conditions associated to each samples (required)

III)	Lane information (required)
