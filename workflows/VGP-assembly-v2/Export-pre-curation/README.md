# Export Pre-curation Assembly Workflow

This workflow exports pre-curation assembly data from the [PretextMap Generation](../hi-c-contact-map-for-assembly-manual-curation/) workflow to a GenomeArk-structured directory tree with VGP naming conventions.

## Overview

The workflow takes outputs from the Hi-C contact map / precuration pipeline (assembly files, tracks, pretext maps, alignments, and QC evaluations) and exports them to a remote destination with standardized paths and file names. Most dataset inputs are auto-populated via tag filters.

## Directory Structure

Files are exported to the following structure:

```
species/{SpeciesName}/{AssemblyID}/assembly_curated/intermediates/
├── {AssemblyID}.HiC.combined.decontam.{Date}.fasta.gz                (merged haplotypes)
├── {AssemblyID}.HiC.hap1.decontam.{Date}.fasta.gz                    (haplotype 1)
├── {AssemblyID}.HiC.hap2.decontam.{Date}.fasta.gz                    (haplotype 2)
│
├── tracks/
│   ├── {AssemblyID}.HiC.combined.decontam.{Date}.gaps.bedgraph        (sequence gaps)
│   ├── {AssemblyID}.HiC.combined.decontam.{Date}.coverage.bigwig      (genome coverage)
│   ├── {AssemblyID}.HiC.combined.decontam.{Date}.telomeres.bed        (telomeres)
│   ├── {AssemblyID}.HiC.combined.decontam.{Date}.coverage_gaps.bedgraph (coverage gaps)
│   ├── {AssemblyID}.HiC.combined.decontam.{Date}.p_telomeres.bed      (P-arm telomeres)
│   ├── {AssemblyID}.HiC.combined.decontam.{Date}.q_telomeres.bed      (Q-arm telomeres)
│   └── {AssemblyID}.HiC.combined.decontam.{Date}.compleasm_genes.bedgraph (gene track, conditional)
│
├── pretextmap/
│   ├── {AssemblyID}.HiC.combined.decontam.mapqfilter.{Date}.pretext   (MAPQ-filtered)
│   └── {AssemblyID}.HiC.combined.decontam.multimapping.{Date}.pretext (multimapping)
│
├── alignments/
│   ├── HiC/{AssemblyID}.HiC.combined.decontam.alignment.{Date}.bam   (Hi-C alignment)
│   └── HiFi/{AssemblyID}.HiFi.combined.decontam.alignment.{Date}.bam (HiFi alignment)
│
└── evaluation/
    ├── {AssemblyID}.HiC.combined.decontam.{Date}.telomere_report.tsv  (Teloscope report)
    ├── {AssemblyID}.HiC.combined.decontam.{Date}.pairtools_stats.tsv  (pairtools stats)
    ├── {AssemblyID}.HiC.combined.decontam.{Date}.pairtools_multiqc.html (pairtools MultiQC)
    ├── {AssemblyID}.HiC.combined.decontam.{Date}.hic_dup_stats.tsv    (Hi-C duplication stats)
    ├── {AssemblyID}.HiC.combined.decontam.{Date}.hic_dup_multiqc.html (Hi-C duplication MultiQC)
    └── {AssemblyID}.HiC.combined.decontam.{Date}.hic_dup_stats_raw.tsv (Hi-C duplication raw)
```

## Input Parameters

### Required Parameters

1. **Species Name** (text) — e.g., `Dermatemys_mawii` (no spaces)
2. **Assembly ID** (text) — e.g., `rDerMaw1` (no spaces)
3. **Date** (text) — format: YYYYMMDD
4. **Destination** (directory_uri) — GenomeArk remote destination

### Boolean Parameters

5. **Do you need to compress the assembly fasta files?** — Set to `yes` if the decontaminated assembly files are in fasta format (not fasta.gz)
6. **Upload Annotations?** — Set to `yes` to export the Compleasm Genes track

### Dataset Inputs (auto-populated by tag)

| Input | Format | Tag |
|-------|--------|-----|
| Compleasm Genes track | bedgraph | `gene_bedgraph` (optional) |
| Sequence Gaps Bedgraph | bedgraph | `Gaps` |
| Genome Coverage Bigwig | bigwig | `coverage` |
| Telomeres Bedgraph file | bedgraph | `Telomeres` |
| Pretext (MAPQ-filtered) | pretext | `pretext_all_tracks` |
| Coverage Gaps Bedgraph | bedgraph | `coverage_gaps` |
| Pretext (multimapping) | pretext | `pretext_all_tracks_multimapping` |
| Merged Haplotypes | fasta | `merged_haps` |
| Haplotype 1 | fasta/fasta.gz | `decont_hap1_suffix` |
| Haplotype 2 | fasta/fasta.gz | `decont_hap2_suffix` |
| Hi-C Alignment | bam | `merged_hic_mapping` |
| HiFi Alignment | bam | — |
| P telomeres bed | bed | `p_telomeres` |
| Q telomeres Bed | bed | `q_telomeres` |
| Telomere Report | tabular | `telomere_report` |
| Pairtools Multiqc Stats | tabular | `pairtools_stats` |
| Pairtools MultiQc Plots | html | `pairtools_multiqc_plots` |
| Hi-C duplication stats | tabular | `hic_dup_stats` |
| Hi-C duplication stats MultiQc | html | `hic_dup_multiqc` |
| Hi-C duplication stats Raw | tabular | `hic_dup_stats_raw` |

## Workflow Structure

The workflow uses three separate export operations:

1. **Precuration files** — Assembly files, tracks, pretext maps, alignments, P/Q telomeres (always runs)
2. **Evaluation files** — Hi-C QC stats and Telomere Report (always runs)
3. **Gene tracks** — Compleasm Genes track (conditional on "Upload Annotations?")

A **TreevalGal Path creation** subworkflow handles directory path construction, creating the `tracks/`, `pretextmap/`, `alignments/`, and `evaluation/` subdirectory paths.

## Companion Workflow

The precuration pipeline that produces the inputs for this export workflow is:
- **[PretextMap Generation from 1 or 2 haplotypes](../hi-c-contact-map-for-assembly-manual-curation/)** (v2.2+)

## Author

Created by: Delphine Lariviere (ORCID: 0000-0001-6421-3484)
License: MIT
