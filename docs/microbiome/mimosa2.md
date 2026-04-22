# MIMOSA2

!!! note "Coming soon"
    Runner not wired yet. R-bridge (`mimosa2::run_mimosa2()`) required
    before this module can execute.

**MIMOSA2** (Model-based Integration of Metabolite Observations and
Species Abundances) is the closest peer to GeMMA's punch framework in
the literature: it computes a per-metabolite "Community-level Metabolic
Potential" (CMP) score from taxon abundance × per-species
reference-genome reactions, then regresses the measured metabolite on
CMP to determine which metabolites are *explained by* the microbiome
and which taxa drive the prediction.

## Comparator positioning vs GeMMA

| Axis                    | GeMMA (punch)                                 | MIMOSA2 (CMP)                                         |
| ----------------------- | --------------------------------------------- | ----------------------------------------------------- |
| Modelling layer         | Genome-scale metabolic models (AGORA2 / VMH)  | Per-species reference reactions (KEGG / AGORA2)       |
| Direction of scoring    | Taxon-centric (which taxa punch above weight) | Metabolite-centric (which metabolites are microbiome-governed) |
| Classifier categories   | driver / passenger / protective / depleted    | microbiome-governed / residual / disagreeing          |
| Requires metabolomics   | Yes (for punch weighting + concordance)       | Yes (for CMP-vs-observed regression)                  |

Running both on the same cohort gives the paper one taxon-first and one
metabolite-first view of the *same* community↔metabolome coupling.

## Required inputs

- Microbiome abundance table (this dataset).
- Paired metabolomics table (auto-resolved via the project's sibling
  metabolomics dataset, same mechanism as GeMMA).
- MIMOSA2 reference reaction DB — pulled from AGORA2 / KEGG; already
  staged by GeMMA, so can be reused without extra downloads.

## Planned outputs

- Per-metabolite CMP-vs-observed scatter + per-taxon contributor
  ranking.
- MIMOSA2's microbiome-governed / residual / disagreeing classifier
  plotted next to GeMMA's driver / passenger / protective / depleted
  so users can see how both framings label the same samples.
