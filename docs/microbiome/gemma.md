# GeMMA

**G**enome-scale community **m**odelling for **m**icrobiome–metabolome
**a**nalysis. GeMMA stitches organism-specific SBML models from
[APOLLO](https://www.vmh.life/) into a sample-weighted community model,
projects each reaction's mapped metabolite set onto your metabolomics
data, and runs a suite of differential / concordance analyses.

## Inputs (auto-paired)

- **Host metabolomics** — any dataset in the project with
  `data_type=metabolomics`. HMDB annotations are ideal but not required;
  GeMMA synthesises HMDB from `KEGG` / `pubchem` / `name` columns on the
  fly when an HMDB column isn't present.
- **Microbiome** — any `metagenomics` dataset in the same project. GeMMA
  prefers a raw MetaPhlAn table (attach as role `metaphlan_species` or
  `metaphlan_raw`) over a genus-rolled preprocessed matrix, because the
  species-first path enables per-strain model resolution.
- **Group comparison (required)** — column name + two distinct values
  selected from the metabolomics sample metadata.

When you land on the GeMMA module from the Microbiome portal, the
paired metabolomics dataset is auto-resolved; you don't pick it.

## What you get

### Concordance vs Activity scatter
Pathway-level scatter: community-model **capacity** on X, metabolomics
**activity** on Y. Four labels / colours:

- **active** — both high (model predicts the pathway; metabolites are elevated).
- **silent** — capacity present, activity missing.
- **exogenous** — activity high without corresponding model capacity.
- **absent** — both low.

Labels are derived from a point's plotted position, so the colour
always matches the quadrant. `± SD` whiskers are off by default —
toggle them from the plot's top-right button.

### Differential pathway enrichment
One ORA run using metabolites elevated in group A vs B as the
foreground (Mann-Whitney per feature, BH-FDR ≤ 0.2 by default). Shown
as a bubble plot (fold-enrichment × −log₁₀(p_adj), size ∝ overlap).

### Punch quadrant
Four-category plot per taxon:

- **Driver** — enriched + metabolically more active in group A.
- **Passenger** — enriched but not more active (went along for the ride).
- **Protective** — depleted but more active per-capita (keystone loss).
- **Depleted** — depleted AND less active.

`secretome ρ` in the table is the per-taxon Spearman correlation
between taxon abundance and the taxon's secretome-metabolite levels —
values near zero are normal for the gut (redundant secretome) and
validate that the punch signal comes from community-weighted scoring,
not organism-specific biomarkers.

### Steiner subnetwork
The *minimally-connected* reaction↔metabolite subgraph linking every
measured metabolite in the community, with edges weighted by community
reaction abundance. Transport / exchange / demand / biomass /
extracellular / diffusion / ABC-transporter subsystems are excluded
from the underlying graph; canonical currency metabolites (ATP, NAD,
H₂O, CoA, Pi, O₂, CO₂, …; sourced from GIZMO's
`KNOWN_CURRENCY_CHEBI` with a hardcoded VMH fallback) are also
excluded. Layout picker + top-N-by-degree filter in the UI manage
density.

### Summary table
Wide table: per pathway, active / silent / exogenous / absent counts
across samples, plus the dominant label. Downloadable CSV.

## Save, share, re-run

- **Save** (top-right, blue) prompts for a name and stores the run.
- **Recent analyses** dropdown on the left panel loads any past run —
  including unsaved ones (unsaved runs are protected by a navigate-away
  confirm dialog).
- Changing only `min_active_fraction` / `min_subnetwork_score` uses a
  fast re-label pass against the cached community; changing anything
  else triggers a full re-run.

## Reproducibility knobs

All captured on the analysis record:

- `random_seed` (default `42`) — seeds the permutation p-values.
- `min_abundance`, `max_models_per_taxon` — slim() filters.
- Pinned VMH snapshot (SHA-verified on load).

## Related manuscript outputs

Inside the app:

- Case-study guides in `data/resources/demo/<study>/guide.html`.

From the command line (see [Benchmark → figure][bench]):

- `python manage.py benchmark_gemma` — 12-study results CSV.
- `python manage.py ablate_gemma --study "…"` — sensitivity sweep.
- `python manage.py plot_benchmark --which main|ablation|humann3` — SVG/PDF figures.

[bench]: ../stats.md
