# MACADAM

!!! note "Coming soon"
    The runner isn't wired yet. Module slot is reserved in the Microbiome
    portal so the left-nav shape stays stable once it ships.

**MACADAM** ("MetAboliC pAthways DAtabase for Microorganisms") rolls a
per-sample taxonomic abundance table into per-pathway completeness
scores by intersecting each observed taxon with pre-computed MetaCyc,
KEGG, and FAPROTAX pathway annotations for the matched reference genome.

## Comparator positioning

MACADAM sits next to HUMAnN3 as a functional-prediction comparator, but:

- **HUMAnN3** uses read depth → gene → pathway abundance.
- **MACADAM** uses taxonomic abundance × per-genome pathway completeness.
- **GeMMA** uses taxonomic abundance × genome-scale metabolic model × metabolomics.

Using all three on the same cohort gives the paper three independent
functional readouts from increasingly model-heavy assumptions.

## Required inputs

- Per-sample taxonomic abundance table (already on every preprocessed
  metagenomics dataset).
- MACADAM reference release — pinned to a semver in the same manifest
  pattern as `vmh_metabolites.manifest.json`, downloaded lazily to
  `$MACADAM_DB`.

## Planned outputs

- Per-sample pathway-completeness matrix.
- Per-contrast Wilcoxon differential (MACADAM completeness A vs B).
- Shared-axis panel against HUMAnN3 + GeMMA enrichment so pathway-ID
  overlap (where the vocabularies intersect) is visible.
