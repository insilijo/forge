# Multi-Omics Integration

The Multi-Omics module combines two or more datasets from the same project to perform joint analysis. Datasets are linked by a shared sample identifier.

---

## Requirements

- At least two datasets in the same project
- A common sample ID that appears in both datasets (e.g. cell line name, patient ID, sample barcode)
- Both datasets must be preprocessed

If your datasets do not share sample IDs directly, you may need to add a mapping column to your sample metadata before uploading.

---

## Linking datasets

When you open the Multi-Omics module, GrAndMA shows all datasets in the current project and attempts to detect the shared sample ID automatically. If detection fails, use the **Link by** dropdowns to specify which column in each dataset's sample metadata contains the shared identifier.

Samples present in one dataset but not the other are excluded from joint analysis. The intersection size is shown before you proceed.

---

## Joint dimensionality reduction

Projects both datasets into a shared low-dimensional space so that samples can be visualised across omics layers simultaneously.

### Concatenated PCA / UMAP

The simplest approach: feature matrices from both datasets are concatenated column-wise (after scaling each dataset independently) and a single PCA or UMAP is run on the combined matrix.

Use this as a quick check for global co-structure between datasets.

### MOFA — Multi-Omics Factor Analysis

Decomposes both datasets into shared latent factors that explain variance across omics layers. Each factor captures a pattern of co-variation — for example, a factor driven by both high glucose and high GLUT1 expression would implicate glycolytic activity.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Factors | 10 | Number of latent factors to learn |
| Convergence | 1e-6 | Tolerance for training convergence |

Results include: factor scores per sample, feature weights per factor per dataset, and variance explained per factor per dataset.

---

## Cross-omics correlation

Tests pairwise correlations between features from dataset A and features from dataset B. For example: which metabolites correlate with which genes across cell lines?

| Parameter | Default | Description |
|-----------|---------|-------------|
| Method | Spearman | Pearson or Spearman |
| FDR threshold | 0.05 | Significance cutoff after BH correction |
| Top features | 50 | Limit to the N most variable features per dataset to reduce computation |

Results are shown as a cross-correlation heatmap. Significant pairs are listed in a sortable table and can be exported.

---

## Joint differential abundance

Tests features from both datasets simultaneously for association with a metadata variable (group, continuous covariate). Results from both omics layers are shown in a combined volcano plot, coloured by dataset.

---

## Example: CCLE paired analysis

The built-in CCLE demo includes bulk metabolomics and pseudo-bulk scRNA-seq for 188 matched cancer cell lines. To explore:

1. Open the CCLE project from the home page
2. Go to Multi-Omics
3. Both datasets are linked by cell line name automatically
4. Run MOFA to identify transcriptomic–metabolomic co-variation factors
5. Colour the factor score plot by `primary_disease` to see cancer-type structure

## Example: microbiome–metabolome studies

All 14 paired microbiome–metabolome demo studies have microbiome and metabolomics datasets under the same project, linked by sample ID. Use cross-omics correlation to find metabolites that associate with specific bacterial genera.
