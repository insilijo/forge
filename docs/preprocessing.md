# Preprocessing

Preprocessing prepares your raw data for analysis by handling missing values, normalising across samples, and optionally log-transforming and scaling. GrAndMA runs these steps in a fixed order via a configurable pipeline.

## Pipeline order

```
Missingness filtering → Imputation → Normalisation → Log transform → Scaling → Batch correction
```

Each step is optional. The default for a new dataset is: no filtering, no imputation, log2 transform, no scaling, no batch correction.

---

## Missingness filtering

Removes samples or features that have too many missing values before imputation.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Sample threshold | 0.5 | Remove samples missing more than this fraction of features |
| Feature threshold | 0.5 | Remove features missing from more than this fraction of samples |

Set both to 1.0 to skip filtering entirely.

---

## Imputation

Fills remaining missing values after filtering.

| Method | When to use |
|--------|-------------|
| **None** | Data has no missing values, or you want to handle them downstream |
| **Minimum** | Replace each missing value with the half-minimum of that feature. Appropriate for metabolomics data where missing = below detection limit |
| **KNN** | Replace using k nearest neighbour imputation (default k=5). Works well for metabolomics with <20% missingness and structured covariation between features |
| **Mean** | Replace with the column mean. Fast but ignores feature covariance |

For microbiome abundance data, use **none** — zeros are structural (truly absent) not missing.

---

## Normalisation

Adjusts for differences in total signal between samples (e.g. dilution, extraction efficiency). Applied before log transformation.

| Method | When to use |
|--------|-------------|
| **None** | Data is already normalised, or normalisation is inappropriate (e.g. scRNA-seq CPM already normalised) |
| **Probabilistic Quotient (PQN)** | Recommended for metabolomics. Divides each sample by its median ratio to a reference sample, robustly handling dilution differences |
| **Total Ion Count (TIC)** | Divides each sample by its total signal. Simpler than PQN; appropriate when total abundance is expected to be equal |

---

## Log transformation

Reduces the influence of extreme values and makes the data more normally distributed.

| Option | When to use |
|--------|-------------|
| **None** | Data is already log-transformed (check "already log-transformed" during configuration) |
| **log2** | Standard for metabolomics and transcriptomics. Fold changes become additive |
| **log10** | Less common; useful when reporting in log10 units is preferred |
| **ln** | Natural log |

If you ticked "already log-transformed" during dataset configuration, this step is skipped automatically.

---

## Scaling

Centers and/or scales each feature so that all features contribute equally regardless of their absolute magnitude.

| Method | When to use |
|--------|-------------|
| **None** | Features are already on comparable scales, or you want to preserve magnitude differences |
| **Autoscaling (z-score)** | Subtract mean and divide by standard deviation. All features have mean 0, variance 1. Use when you want equal weight for all features regardless of variance |
| **Pareto** | Divide by the square root of the standard deviation. Less aggressive than autoscaling; preserves some magnitude information |

---

## Batch correction

Removes systematic technical variation between batches of samples measured at different times or by different operators. Requires a **batch column** in your sample metadata.

| Method | When to use |
|--------|-------------|
| **None** | No batch variable, or batches are balanced across groups |
| **ComBat** | Empirical Bayes method. Robust for small batch sizes. Can preserve biological covariates (specify in the covariate field) |

Batch correction is applied last, after all other preprocessing steps.

---

## Re-running preprocessing

You can re-run preprocessing with different settings at any time. Go to the preprocessing page for your dataset and adjust the parameters. Previous runs are preserved — you can switch between them using the run selector. Only one run is active at a time and all downstream analyses use the active run.

---

## Viewing results

After preprocessing, the **View Data** page shows:

- Sample count and feature count after filtering
- Missing value heatmap before and after imputation
- Sample distribution (box plot) before and after normalisation
- PCA score plot coloured by any metadata column
- Feature variance distribution
