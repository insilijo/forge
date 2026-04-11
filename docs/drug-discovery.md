# Drug Discovery Pipeline

GrAndMA includes a two-stage drug discovery pipeline that takes GIZMO disease-metabolite-gene scoring as input and produces ranked small-molecule fragment candidates.

The pipeline runs within a single dataset context and is designed to be run sequentially: Ducky → Chips.

---

## Stage 1: Ducky — Target Ranking

Ducky takes disease-associated gene targets surfaced by the GIZMO knowledge graph (and optionally filtered by Discomarker SHAP candidates) and scores each target across four dimensions to produce a ranked target manifest.

### Scoring dimensions

| Dimension | Description |
|-----------|-------------|
| Tractability | Druggability assessment: small molecule, antibody, and PROTAC tractability tiers |
| Pocket | Binding pocket quality: volume, druggability score, crystallographic evidence |
| Selectivity | Paralog landscape: how many closely related targets exist and how distinct is the binding site |
| Landscape | Clinical and competitive landscape: number of approved drugs, pipeline compounds, patents |

### Input

- A completed GizmoRun on the same dataset (required)
- Optionally: a completed DiscomarkerRun — restricts targets to SHAP top features

### Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Top N targets | 20 | Maximum number of targets to score |

### Output

- Ranked target manifest with tier assignments per scoring dimension
- Tier summary table

---

## Stage 2: Chips — Fragment Search and ADMET

Chips takes the target manifest from a completed DuckyRun and runs fragment-based candidate search, ADMET filtering, and synthetic accessibility scoring.

### Pipeline steps

1. **Fragment search** — searches a fragment library against each target's binding site
2. **ADMET triage** — filters candidates on Lipinski Ro5, logP, TPSA, hERG liability, and solubility
3. **SA score** — estimates synthetic accessibility for each surviving candidate

### Parameters

| Parameter | Description |
|-----------|-------------|
| Skip ADMET | Bypass ADMET filtering (useful for exploratory runs) |

### Output

- Candidate table with SMILES, target, fragment scores, ADMET profile, and SA score
- Summary counts: total fragments, ADMET pass rate, final candidates

---

## Typical workflow

1. Run GIZMO to score metabolite–disease–gene connections
2. (Optional) Run Discomarker to identify SHAP-ranked biomarker features
3. Run Ducky — select a GizmoRun (and optionally a DiscomarkerRun) as inputs; review ranked targets
4. Run Chips — select the DuckyRun as input; review fragment candidates and ADMET-filtered hits
