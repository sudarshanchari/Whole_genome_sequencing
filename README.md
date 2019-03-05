Analysis of pooled, whole genome resequencing data via mapping to a reference
=============================================================================

This Repository contains a walk-through for analyzing pooled, (paired-end) Illumina WGS performed on population of Drosophila melanogaster (whole) adults that were evolved under specific conditions for 30+ generations. The final objective of the analysis is to understand the genetic changes underlying the evolution of specific traits. For more deatils, please see https://www.biorxiv.org/content/10.1101/200725v1.abstract 

Specifically this repository contains:
- pre-processing raw reads and mapping to a reference genome using a BWA-MEM and Novoalign

- processing post mapping viz sorting and merging using SAMtools, marking duplicates using Picard, indel realignment using GATK and variant calling using SAMTools/ bcftools 

- testing for changes in allele frequencies and calculating population genetic parameters as well as custom plotting/ visualization in R
