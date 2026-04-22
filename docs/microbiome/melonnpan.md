# MelonnPan

!!! note "Coming soon"
    Runner not wired yet. R-bridge (`MelonnPan::melonnpan.predict()`)
    required before this module can execute.

**MelonnPan** predicts metabolite levels from microbial community
composition by fitting an elastic-net regression per metabolite against
microbiome features on a reference cohort, then applying the trained
model to a new sample's microbiome. The prediction is microbiome-only —
no metabolomics required at inference time.

## Comparator positioning

MelonnPan is the inference-from-statistics counterpart to GeMMA's
inference-from-mechanism. Both output predicted metabolite profiles
from microbiome composition, but:

- **MelonnPan** learns a regression model from a training cohort.
- **GeMMA** derives predictions from community-weighted GSMM pathway
  capacity — no training, reconstruction-first.

Paired with observed metabolomics, both can be validated by predicted
vs measured scatter (per-metabolite R² / Spearman ρ). A study where
GeMMA matches observed better than MelonnPan argues for mechanism-first
framing; the reverse argues for training-data dependence.

## Required inputs

- Microbial feature abundance table (preprocessed matrix).
- A published MelonnPan reference bundle (the Borenstein group
  distributes several; HMP, IBD, etc.).
- Target metabolite namespace — HMDB to align with GeMMA's synthesis
  chain and with the platform's global metabolite vocabulary.

## Planned outputs

- Predicted metabolite table (rows × samples).
- Per-sample predicted-vs-observed scatter (R² + Spearman ρ).
- Per-metabolite heatmap across samples.
- Optional leave-one-out retrain when group sizes permit.
