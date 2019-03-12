Analysis of pooled, whole genome resequencing data via mapping to a reference
=============================================================================

This Repository contains a walk-through/ scripts for analyzing pooled, (paired-end) Illumina WGS performed on population of Drosophila melanogaster (whole) adults that were evolved under specific conditions for 30+ generations. The final objective of the analysis is to understand the genetic changes underlying the evolution of specific traits. For more deatils on the evolution procedure and traits evolved, please see https://www.biorxiv.org/content/10.1101/200725v1.abstract 

Specifically this repository contains methods/ scripts for:
- pre-processing raw reads and mapping to a reference genome using a BWA-MEM and Novoalign
  - see https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5100849/ for the reason for using multiple mapping algorithms
  

- processing post mapping viz sorting and merging using SAMtools, marking duplicates using Picard, indel realignment using GATK and variant calling using SAMTools/ bcftools 
  - GATK cannot be used for variant calling of pooled data, for example see https://gatkforums.broadinstitute.org/gatk/discussion/6520/pooled-dna-sequencing-variant-calling-using-haplotypecaller
  - While GATK version4 has eliminated the need for the indel realignment step, if you are using other variant callers, it is recommended to perform indel realignment
  

- testing for changes in allele frequencies and calculating population genetic parameters as well as custom plotting/ visualization in R
  - Popoolation and vcftools can be used to do many of the above calculations 

## Computing Environment and Job Scheduling
Please see my repository on how I typically do this https://github.com/sudarshanchari/Computing_Environment_Job_Scheduling
  
## Adapter trimming
I have extensively used Trimmomatic (http://www.usadellab.org/cms/?page=trimmomatic) and Trim Galore (https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/). Both of these tools are easy to use and once again for a simple case, I haven't seen a qualitative difference between the two. 
```
# source activate ngs
# conda install trim-galore

### TrimGalore example
# For paired end reads
trim_galore --paired filename_Read1.fastq.gz filename_Read2.fastq.gz -o folder_name

# TrimGalore automatically prints out filename within the output folder based on the input filename
# TrimGalore also autodetects the adapters to be trimmed based on the 1st million reads

### Trimmomatic example
# For paired reads
trimmomatic PE filename_Read1.fastq filename_Read2.fastq filename_Read1_pe.fastq filename_Read1_se.fastq filename_Read2_pe.fastq filename_Read2_se.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10:4 

# Initially Trimmomatic will look for seed matches (16 bases) allowing maximally 2 mismatches. These seeds will be extended and clipped if in the case of paired end reads a score of 30 is reached (~50 bases), or in the case of single ended reads a score of 10(~17 bases). Also the minimum adapter length that it can detect will be 4 (instead of the default 8)

# Additional parameters that can be added 
LEADING:3 TRAILING:3 MAXINFO:40:0.4 MINLEN:50 

# Remove leading low quality or N bases (below quality 3)
# Remove trailing low quality or N bases (below quality 3)
# MAXINFO:<targetLength>:<strictness> 
  # targetLength: This specifies the read length which is likely to allow the location of the read within the target sequence to be determined. The reads need to be sufficiently long so that they can be uniquely mapped. The Trimmomatic manual/ reference says this should be in the order of 40 bases- that I have used. 
  # strictness: This value, which should be set between 0 and 1, specifies the balance between preserving as much read length as possible vs. removal of incorrect bases. A low value of this parameter (<0.2) favours longer reads, while a high value (>0.8) favours read correctness. The Trimmomatic reference has used 0.4.

# Unlike SLIDINGWINDOW there are no established parameters for MAXINFO although the manual/ reference says MAXINFO should theoretically outperform SLIDINGWINDOW.

# Finally, the MINLEN:50 will remove all reads less than 50bp. This is necessary for the mapping step, where bwa mem works best with reads greater than 50bp. Although, not necessary.

# Check the trimmed sequences for discrepancies in the flags. If everything looks fine, please proceed.

```

