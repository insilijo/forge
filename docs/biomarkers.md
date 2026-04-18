# Biomarker Discovery

GrAndMA includes two modules for identifying and validating biomarkers from omics data.

---

## Discomarker

Discovers candidate biomarkers using regularised regression with SHAP-based feature importance and optional consensus clustering.

### Requirements

- A preprocessed dataset
- A sample metadata column to use as the outcome variable (binary class label or continuous value)

### How it works

1. Loads the preprocessed feature matrix and merges the selected outcome column from sample metadata
2. Fits an elastic net model with cross-validation to select features predictive of the outcome
3. Computes SHAP values to rank features by their contribution to the model
4. Optionally runs consensus clustering across a range of *k* to identify sample subtypes

### Parameters

| Parameter | Description |
|-----------|-------------|
| Outcome column | Metadata column to predict (binary or continuous) |
| Group column | Optional stratification variable for CV splits |
| Top N features | Number of top SHAP features to retain for downstream analysis |

### Results

- **SHAP ranking** — features ordered by mean absolute SHAP value
- **Model metrics** — cross-validated performance (AUC for classification, R² for regression)
- **Cluster labels** — sample subtype assignments (if clustering enabled)
- **Top feature table** — candidates for follow-up validation (MetaSurv, orthogonal assays, manual review)

---

## MetaSurv

Survival analysis module for time-to-event endpoints.

### Requirements

- A preprocessed dataset
- Sample metadata with a time column and an event column (0/1)

### Analyses

| Analysis | Description |
|----------|-------------|
| Cox proportional hazards | Fits a regularised Cox model on omics features; reports C-index (cross-validated) and hazard ratios |
| Kaplan-Meier | Stratified KM curves for a grouping variable |
| Competing risk | Fine-Gray model for competing events (e.g. death from other causes) |
| Landmark analysis | Conditional survival from a landmark time point; corrects for guarantee-time bias |

### Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| Time column | — | Sample metadata column with follow-up time |
| Event column | — | Sample metadata column with event indicator (1 = event, 0 = censored) |
| CV folds | 5 | Cross-validation folds for Cox model evaluation |

### Results

- Kaplan-Meier plot with log-rank p-value
- Cumulative incidence curves (competing risk)
- Landmark survival curves
- Cox coefficients table, C-index, and feature importance ranking
