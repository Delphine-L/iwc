# Export Curated Assembly Workflow

This workflow exports curated assembly data with separate outputs for haplotype 1 and haplotype 2, following the VGP directory structure.

## Overview

The workflow takes curated assembly files (tracks, pretext maps, and alignments) for both haplotypes and exports them to a structured directory tree with proper VGP naming conventions.

## Directory Structure

Files are exported to the following structure:

```
species/{SpeciesName}/{AssemblyID}/assembly_curated/curated/
├── hap1/
│   ├── tracks/
│   │   ├── {AssemblyID}.hap1.cur.{Date}.gaps_frac.bigwig
│   │   ├── {AssemblyID}.hap1.cur.{Date}.gaps_count.bigwig
│   │   ├── {AssemblyID}.hap1.cur.{Date}.coverage.bigwig
│   │   └── {AssemblyID}.hap1.cur.{Date}.telomeres.bed
│   ├── pretext/
│   │   └── {AssemblyID}.hap1.cur.{Date}.pretext
│   └── alignments/
│       ├── HiC/
│       │   └── {AssemblyID}.hap1.cur.{Date}.HiC.bam
│       └── HiFi/
│           └── {AssemblyID}.hap1.cur.{Date}.HiFi.bam
└── hap2/
    ├── tracks/
    │   ├── {AssemblyID}.hap2.cur.{Date}.gaps_frac.bigwig
    │   ├── {AssemblyID}.hap2.cur.{Date}.gaps_count.bigwig
    │   ├── {AssemblyID}.hap2.cur.{Date}.coverage.bigwig
    │   └── {AssemblyID}.hap2.cur.{Date}.telomeres.bed
    ├── pretext/
    │   └── {AssemblyID}.hap2.cur.{Date}.pretext
    └── alignments/
        ├── HiC/
        │   └── {AssemblyID}.hap2.cur.{Date}.HiC.bam
        └── HiFi/
            └── {AssemblyID}.hap2.cur.{Date}.HiFi.bam
```

## Input Parameters

### Required Parameters

1. **Species Name** (text)
   - Example: `Dermatemys_mawii`
   - Format: No spaces allowed

2. **Assembly ID** (text)
   - Example: `rDerMaw1`
   - Format: No spaces allowed

3. **Date** (text)
   - Example: `20251106`
   - Format: YYYYMMDD (8 digits)

4. **Root Directory for Export** (directory_uri)
   - The remote destination for exported files
   - Files will be exported to the `species/` folder within this directory

### Dataset Inputs

#### Haplotype 1 (7 datasets)

1. **Gaps Bigwig Hap1 (frac)** - format: bigwig, tag: `gaps_frac_hap1`
2. **Gaps Bigwig Hap1 (count)** - format: bigwig, tag: `gaps_count_hap1`
3. **Genome Coverage Hap1** - format: bigwig, tag: `coverage_hap1`
4. **Telomeres Hap1** - format: bed, tag: `telomeres_hap1`
5. **Pretext Hap1** - format: pretext, tag: `pretext_hap1`
6. **HiC Alignment Hap1** - format: bam, tag: `hic_bam_hap1`
7. **HiFi Alignment Hap1** - format: bam, tag: `hifi_bam_hap1`

#### Haplotype 2 (7 datasets)

1. **Gaps Bigwig Hap2 (frac)** - format: bigwig, tag: `gaps_frac_hap2`
2. **Gaps Bigwig Hap2 (count)** - format: bigwig, tag: `gaps_count_hap2`
3. **Genome Coverage Hap2** - format: bigwig, tag: `coverage_hap2`
4. **Telomeres Hap2** - format: bed, tag: `telomeres_hap2`
5. **Pretext Hap2** - format: pretext, tag: `pretext_hap2`
6. **HiC Alignment Hap2** - format: bam, tag: `hic_bam_hap2`
7. **HiFi Alignment Hap2** - format: bam, tag: `hifi_bam_hap2`

## File Naming Convention

Files follow the VGP naming standard:

```
{AssemblyID}.{Haplotype}.cur.{Date}.{Description}.{Extension}
```

Where:
- **AssemblyID**: The assembly identifier (e.g., `rDerMaw1`)
- **Haplotype**: Either `hap1` or `hap2`
- **cur**: Stage identifier for "curated"
- **Date**: Date in YYYYMMDD format
- **Description**: File type descriptor (e.g., `gaps_frac`, `coverage`, `HiC`)
- **Extension**: File format extension (e.g., `.bigwig`, `.bed`, `.bam`, `.pretext`)

### Examples

- `rDerMaw1.hap1.cur.20251106.gaps_frac.bigwig`
- `rDerMaw1.hap2.cur.20251106.coverage.bigwig`
- `rDerMaw1.hap1.cur.20251106.HiC.bam`
- `rDerMaw1.hap2.cur.20251106.pretext`

## Workflow Structure

The workflow uses a **subworkflow architecture** for cleaner path management:

### Main Workflow Steps (35 total)

1. **Steps 0-3**: Input parameters (Species Name, Assembly ID, Date, Root Directory)
2. **Steps 4-17**: Dataset inputs (7 for hap1, 7 for hap2)
3. **Step 18**: **Curated Paths Creation subworkflow** - Creates all directory paths
4. **Steps 19-32**: File name composition (7 for hap1, 7 for hap2)
5. **Steps 33-34**: Export steps (one for each haplotype)

### Curated Paths Creation Subworkflow (13 steps)

This embedded subworkflow handles all path construction:

1. **Steps 0-1**: Inputs (Species Name, Assembly ID)
2. **Step 2**: Root path creation (`species/{Name}/{ID}/assembly_curated/curated/`)
3. **Steps 3-4**: Haplotype base paths (`hap1/`, `hap2/`)
4. **Steps 5-8**: Hap1 subdirectories (`tracks/`, `pretext/`, `alignments/HiC/`, `alignments/HiFi/`)
5. **Steps 9-12**: Hap2 subdirectories (same structure)

**Benefits of subworkflow approach:**
- Cleaner separation of concerns
- Reusable path logic
- Easier to maintain and update
- Matches VGP workflow conventions

## Usage Notes

- **Separate exports**: The workflow creates two separate export operations, one for each haplotype
- **Path validation**: Input validators ensure species name and assembly ID don't contain spaces
- **Date format**: Date must be exactly 8 digits in YYYYMMDD format
- **File tags**: Use dataset tags when preparing inputs to ensure correct file assignment

## Differences from Export-pre-curation

| Feature | Export-pre-curation | Export Curated |
|---------|---------------------|----------------|
| Directory | `intermediates/` | `curated/hap1/` and `curated/hap2/` |
| Haplotypes | Combined in single files | Separate files per haplotype |
| Alignments | Not exported | HiC and HiFi BAM files exported |
| Stage ID | None | Uses "cur" identifier |
| File organization | Flat structure | Organized by data type (tracks, pretext, alignments) |

## Generation

This workflow was generated using `generate_workflow_with_subworkflow.py`. To regenerate:

```bash
cd workflows/VGP-assembly-v2/Export-curated
python3 generate_workflow_with_subworkflow.py
```

**Note:** An older version (`generate_workflow.py`) creates the workflow without the subworkflow structure. The subworkflow version is recommended as it follows VGP conventions.

## Validation

To validate the workflow JSON:

```bash
python3 -m json.tool Galaxy-Workflow-Export_Curated.ga > /dev/null && echo "Valid"
```

## Author

Created by: Delphine Lariviere (ORCID: 0000-0001-6421-3484)
License: MIT
