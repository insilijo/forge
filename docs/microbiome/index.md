# Microbiome

The **Microbiome** section is a portal that hosts every module that
operates on microbiome data — community metabolic modelling, pathway
differential abundance, feature-association modelling, and (over time)
more. Each module is independently executable; you pick the one that
answers the question you actually have, without being forced through
another one's configuration.

## When to use this section

When you have at least one **metagenomics** dataset in a project
(MetaPhlAn shotgun, or an upload whose `data_type` is set to
`metagenomics` during configure). A paired **metabolomics** dataset in
the same project unlocks GeMMA; the other modules don't require
pairing.

## The module portal

Clicking **Microbiome** in the top nav lands you on the portal for the
project's first metagenomics dataset. The left panel lists all
available modules with one-line descriptions:

| Module    | What it does                                                                  | Requires                                                |
| --------- | ----------------------------------------------------------------------------- | ------------------------------------------------------- |
| GeMMA     | Community genome-scale metabolic model + punch differential + concordance.    | Paired metabolomics dataset (HMDB-annotated preferred). |
| HUMAnN3   | MetaCyc pathway abundance from a pre-computed HUMAnN3 run; Wilcoxon per path. | Attached `pathabundance_cpm.tsv` (role `humann3_pathabundance`). |
| MaAsLin2  | General linear (or linear-mixed) model for microbial feature ↔ metadata.      | *Coming soon — runner not wired yet.*                   |

Modules are greyed out when their prerequisites aren't met — hover for
the reason.

## How modules relate

Each module writes its own database record (`GemmaAnalysis`,
`Humann3Analysis`, `MaaslinAnalysis`) and has its own results page.
There is no shared "run everything" button; modules don't depend on
each other's outputs. You can compare their outputs side-by-side using
[Compare](../stats.md#compare-datasets) when both modules use
overlapping pathway or taxon identifiers.

## Module pages

- [GeMMA](gemma.md) — community metabolic model, punch, concordance, Steiner subnetworks.
- [HUMAnN3](humann3.md) — MetaCyc pathway differential (FASTQ → `pathabundance` → Wilcoxon).
- [MaAsLin2](maaslin.md) — microbial feature association modelling (placeholder).

## Reproducibility

Every module run captures the input file identity, group-comparison
parameters, and (for GeMMA) the random seed used for permutation
statistics. The pinned VMH metabolite snapshot
(`data/resources/gemma/apollo/vmh_metabolites.csv`) carries a
manifest + SHA256 so the HMDB↔VMH↔KEGG mapping is byte-identical
across hosts and dates — required for publication-grade reruns.
