# MetaProSIP: automated inference of elemental fluxes in microbial communities

## Inputs dataset

- `Centroided LC-MS datasets` in mzML (MetaProSIP is mainly tested on data generated by orbitrap instruments)
- `Fasta Database` in Fasta (aminoacid sequences)

## Inputs values

- `Precursor monoisotopic mass tolerance` (ppm): This value is passed to
  - MSGFPlusAdapter parameter `Precursor monoisotopic mass tolerance` (-precursor_mass_tolerance)
  - MetaProSIP parameter `Tolerance in ppm` (-mz_tolerance_ppm)
- Fixed modifications
- Variable modifications
- Labeled element

## Processing

- DecoyDatabase: Add decoy sequences to the Fasta database (for FDR calculation)
- FeatureFinderCentroided: identify eluting peptides that correspond to isotopologues with natural isotopic distributions 
- MSGFPlusAdapter: identify peptides through peptide fragment fingerprinting (database search)
- FeatureFinderMultiplex: detect elution profiles of unlabeled peptides
- PeptideIndexer: annotate protein association to identified peptides
- FalseDiscoveryRate: Calculate FDR
- IDMapper: map identified spectra to elution profiles
- MetaProSIP: calculate the protein-SIP features, to perform functional grouping, and for protein inference