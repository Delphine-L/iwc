# Changelog

## [1.0] - 2026-03-24

### Added
- Export of P and Q telomere BED tracks
- Export of Telomere Report
- Export of Hi-C QC stats (Pairtools stats, Pairtools MultiQC, duplication stats, duplication MultiQC, duplication raw)
- New `evaluation/` subdirectory for QC and evaluation outputs
- Compleasm Genes track export (conditional, replacing separate hap1/hap2 miniprot GFFs)

### Changed
- Gene annotation export now uses single merged Compleasm Genes track (bedgraph) with tag filter `gene_bedgraph`
