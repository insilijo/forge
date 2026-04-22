# MaAsLin2

!!! note "Coming soon"
    The MaAsLin2 runner isn't wired yet. The module slot is reserved so
    the Microbiome portal's left-hand nav stays stable once the runner
    ships — analysis records can still be created and saved through the
    admin in the meantime.

## What it will do

MaAsLin2 (Multivariate Association with Linear Models) fits
per-feature generalised linear (or linear-mixed) models between microbial
features — taxa, pathways, functions — and sample-level metadata.

Parameters the module already captures on its model, ready for the
runner:

- `fixed_effects` (list): covariates treated as fixed (e.g. `Study.Group`, `Age`).
- `random_effects` (list): grouping variables (e.g. `Subject`, `Site`).
- `reference_levels` (dict): categorical baselines (e.g. `Study.Group → Control`).
- `normalization`: `TSS`, `CLR`, `CSS`, `TMM`, `NONE`. Default `TSS`.
- `transform`: `LOG`, `LOGIT`, `AST`, `NONE`. Default `LOG`.

## Comparator positioning

MaAsLin2 is the natural comparator to GeMMA's punch-table CLR+Wilcoxon
row because it asks the same question (which microbial features shift
with metadata?) using a richer modelling framework: multiple covariates
at once, correct for random effects, and handle compositional data
explicitly.

## Runner bring-up plan

1. Add an R-bridge container (R + `MaAsLin2` package) to `docker-compose.yml`.
2. Extend `MaaslinAnalysis` with fixed/random-effect wiring from the UI.
3. Ship a MaAsLin2 results template analogous to `humann3.html`.
4. Point the CLI comparator (`benchmark_maaslin`) at the same cohort as
   `benchmark_gemma` so the paper's main figure can stack all three
   results side-by-side.
