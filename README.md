# GATK Germline Variant Calling Workflow

A reproducible workflow for **NGS variant discovery** using GATK Best Practices, covering data pre-processing, per-sample variant calling, joint genotyping, and variant filtering.

For example run, use the accession SRR062634. 

Or customize your sample in appropriate positions that are indicated with 'sample' in the script.

## Overview

This repository is designed to present a clean and reusable workflow for calling **germline SNPs and Indels** from high-throughput sequencing data using GATK4. The pipeline follows the general logic described in GATK Best Practices, including duplicate marking, base quality score recalibration, per-sample calling in GVCF mode, joint genotyping, and downstream filtering.

The goal of this repository is to make the workflow easier to understand, adapt, and reuse for research projects that require structured variant discovery from raw or aligned sequencing data.

## Workflow summary

The pipeline can be organized into the following major stages:

1. Raw data quality assessment
2. Read alignment to the reference genome
3. BAM sorting and indexing
4. Duplicate marking
5. Base quality score recalibration (BQSR)
6. Variant calling with `HaplotypeCaller` in GVCF mode
7. Joint genotyping across samples
8. Variant filtering and generation of final VCF files

## Typical workflow

### 1. Pre-processing

GATK Best Practices describe data pre-processing as the required first phase before variant discovery. Core pre-processing steps include duplicate marking and BQSR to reduce technical bias and improve downstream variant calls.

Typical tools:
- `FastQC`
- `BWA`
- `samtools`
- `MarkDuplicates`
- `BaseRecalibrator`
- `ApplyBQSR`

### 2. Per-sample calling

For germline short variant discovery, `HaplotypeCaller` is commonly used to produce GVCF output for each sample. This supports later joint genotyping and makes it easier to add samples progressively.

Typical tool:
- `HaplotypeCaller`

### 3. Joint genotyping

Multi-sample workflows generally combine per-sample GVCFs and perform joint genotyping. GATK workflow examples include `GenomicsDBImport` followed by `GenotypeGVCFs`.

Typical tools:
- `GenomicsDBImport`
- `GenotypeGVCFs`

### 4. Variant filtering

Raw variant calls contain artifacts, so filtering is needed before interpretation. GATK documentation describes variant filtering as a dedicated stage after calling, using approaches such as VQSR where appropriate.

Typical tools:
- `VariantRecalibrator`
- `ApplyVQSR`
- Hard filtering, when VQSR is not appropriate for the dataset

## Input requirements

Prepare the following before running the workflow:

- Paired-end or single-end sequencing reads, or coordinate-sorted BAM files
- Reference genome FASTA
- Reference genome index files
- Known-sites variant resources for BQSR, when available
- Sample sheet with sample identifiers and file paths

## Expected outputs

Typical outputs include:

- Aligned and sorted BAM files
- Duplicate-marked BAM files
- Recalibrated BAM files
- Per-sample GVCF files
- Joint genotyped VCF files
- Filtered final VCF files
- Summary metrics and logs

## Software

This workflow is intended for a GATK4-style analysis setup and is typically used alongside commonly adopted tools for alignment, BAM processing, and QC.

Recommended software:
- GATK4
- BWA
- samtools
- Picard tools
- FastQC
- MultiQC

## Reusability notes

To make this repository more reusable for other researchers:

- Keep sample metadata in a tabular file rather than hardcoding paths.
- Store software versions in a separate file.
- Separate configuration, workflow scripts, and documentation.
- Use clear output directories for each stage.
- Add one worked example with expected file names and output descriptions.

## Recommended GitHub description

**Reusable GATK4 workflow for germline SNP/Indel discovery, from pre-processing and BQSR to GVCF calling, joint genotyping, and filtered VCF output.**

## Citation and acknowledgment

This workflow is conceptually aligned with GATK Best Practices and official GATK workflow resources from the Broad Institute.
