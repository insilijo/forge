# HUMAnN3

Per-pathway differential abundance between two sample groups, using
pre-computed HUMAnN3 output. HUMAnN3 itself is the standard
"functional-prediction from shotgun reads" pipeline — we don't bundle
the compute (it's 20+ GB of reference DBs and hours per sample) but
ingest its output directly.

## Inputs

Upload a joined, normalised HUMAnN3 pathway table to the **microbiome**
dataset with role `humann3_pathabundance`:

```bash
# On a compute node with HUMAnN3 installed:
./scripts/run_humann3.sh \
    --fastq-dir /data/erawijantari/fastq \
    --out-dir   /data/erawijantari/humann3_out \
    --threads   8

# Copy the result to the GrAndMA host
scp node:/data/erawijantari/humann3_out/joined/pathabundance_cpm.tsv \
    local:/tmp/
```

Then in the GrAndMA UI: open the microbiome dataset → **Files** →
**Upload**, pick the `pathabundance_cpm.tsv` and set role
`humann3_pathabundance`. The **Microbiome → HUMAnN3** left-panel slot
un-greys.

## Running

Open the HUMAnN3 module from the left nav:

1. Pick **Group column** from the metabolomics dataset's sample metadata.
2. Set **Case (A)** and **Control (B)**.
3. (Optional) adjust the BH-FDR threshold — default `0.1`.
4. Click **Run HUMAnN3 differential**.

The runner:

- Drops HUMAnN3's stratified rows (`... |g__Genus.s__species`) and
  `UNMAPPED`/`UNINTEGRATED` aggregates — both would distort per-pathway
  statistics.
- Runs Mann-Whitney U per pathway between samples in group A and
  group B, BH-adjusts within the run.
- Emits a bubble plot + sortable results table.

## Outputs

- **Pathway table** — ranked by BH-FDR ascending. Columns:
    `pathway`, `lfc_a_over_b` (log₂ fold-change, positive = up in A),
    `p_value`, `p_adj`, `significant` (bool at the chosen FDR).
- **Bubble plot** — one point per pathway, size ∝ overlap, colour ∝
  significance at your FDR.
- **Diagnostics strip** — `n_pathways`, `n_significant`, group labels,
  FDR threshold.

## Reproducibility

- The analysis record captures: `source_filename`, `group_column`,
  `group_a`, `group_b`, `fdr_threshold`, created_at, created_by.
- HUMAnN3 itself is deterministic given the same reference DB; pin the
  `chocophlan` / `uniref90_diamond` version in your compute node's
  `$HUMANN_DB` directory and record that in the manuscript's methods
  section.

## Comparing with GeMMA

The GeMMA **Differential Enrichment** tab and the HUMAnN3 module both
produce pathway-level differentials — but:

- **GeMMA** uses the community metabolic model's subsystem → VMH
  metabolite sets, then runs ORA on *metabolomics*-derived
  A-vs-B-elevated HMDBs.
- **HUMAnN3** uses gene-family abundance → MetaCyc pathway abundance,
  then runs Wilcoxon per pathway on the *HUMAnN3 table*.

Different vocabularies (GeMMA's AGORA2 subsystems vs HUMAnN3's MetaCyc
pathway IDs) mean literal name-overlap is low; the publication-grade
comparison is quantitative (how many pathways each finds significant)
and biological (do the themes agree). The `benchmark_humann3`
management command writes a CSV keyed to the same cohort so this
comparison can be rendered as a supplementary figure.

## CLI equivalent

For reproducible benchmarking outside the UI:

```bash
docker exec grandma-app python manage.py benchmark_humann3 \
    --study "Erawijantari Gastric Cancer 2020" \
    --pathabundance /path/pathabundance_cpm.tsv \
    --group-column Study.Group --group-a Gastrectomy --group-b Healthy \
    --out reports/humann3_erawijantari.csv

docker exec grandma-app python manage.py plot_benchmark \
    --which humann3 --in reports/humann3_erawijantari.csv \
    --out reports/figures/ --tag erawijantari
```
