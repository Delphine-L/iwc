# VGP Workflow #1

This workflow collects the metrics on the properties of the genome under consideration by analyzing the *k*-mer frequencies. It provides information about the genomic complexity, such as the genome size and levels of heterozygosity and repeat content, as well about the data quality. It uses reads from two parental genomes to partition long reads from the offspring into haplotype-specific *k*-mer databases.

### Inputs

1. Name of the Species
2. Name of the assembly
3. Collection of Hifi long reads [fastq] (Collection)
4. Paternal short-read Illumina sequencing reads [fastq] (Collection)
5. Maternal short-read Illumina sequencing reads [fastq] (Collection)
6. *k*-mer length
7. Ploidy

### Outputs

-   Meryl databases of k-mer counts
    - Child
    - Paternal haplotype
    - Maternal haplotype
-   GenomeScope metrics for child and the two parental genomes (three GenomeScope profiles in total)
    -   Linear plot
    -   Log plot
    -   Transformed linear plot
    -   Transformed log plot
    -   Summary
    -   Model
    -   Model parameteres
-   RDeval HiFi reads QC:
    -   Statistics
    -   HTML report
 
    ![image](https://github.com/galaxyproject/iwc/assets/4291636/35282f8e-d021-44f6-8e03-7b58b32d6d00)
