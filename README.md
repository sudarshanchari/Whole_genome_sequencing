Analysis of pooled, whole genome resequencing data via mapping to a reference
=============================================================================

This Repository contains a walk-through for analyzing pooled, (paired-end) Illumina WGS performed on population of Drosophila melanogaster (whole) adults that were evolved under specific conditions for 30+ generations. The final objective of the analysis is to understand the genetic changes underlying the evolution of specific traits. For more deatils on the evolution procedure and traits evolved, please see https://www.biorxiv.org/content/10.1101/200725v1.abstract 

Specifically this repository contains methods/ scripts for:
- pre-processing raw reads and mapping to a reference genome using a BWA-MEM and Novoalign
  - see https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5100849/ for the reason for using multiple mapping algorithms
  

- processing post mapping viz sorting and merging using SAMtools, marking duplicates using Picard, indel realignment using GATK and variant calling using SAMTools/ bcftools 
  - GATK cannot be used for variant calling of pooled data, for example see https://gatkforums.broadinstitute.org/gatk/discussion/6520/pooled-dna-sequencing-variant-calling-using-haplotypecaller
  - While GATK version4 has eliminated the need for the indel realignment step, if you are using other variant callers, it is recommended to perform indel realignment
  

- testing for changes in allele frequencies and calculating population genetic parameters as well as custom plotting/ visualization in R
  - Popoolation and vcftools can be used to do many of the above calculations 
