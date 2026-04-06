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

## Combined Statistics

Runs supervised or unsupervised methods on the concatenated feature matrix from all selected datasets.

### PCA on concatenated matrix

Feature matrices are concatenated column-wise (each dataset scaled independently) and a single PCA is run. Use **Color by** and **Shape by** in Plot Options to annotate samples post-hoc — no group column required at run time.

### PLS-DA on concatenated matrix

Requires a **Group column** (a metadata column shared across datasets) as the response variable. Useful for identifying features that discriminate between groups across omics layers.

---

## Unsupervised Integration

Methods that find shared latent structure across omics layers without requiring a group label. Group colouring is applied post-hoc via **Color by** in Plot Options.

### MCIA — Multiple Co-Inertia Analysis

Finds axes of maximum co-inertia between datasets. Produces joint sample scores (all datasets), per-block sample scores, and a co-inertia-by-component bar chart.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Components | 2 | Number of co-inertia axes to compute |

### JIVE — Joint and Individual Variation Explained

Separates variation into joint structure (shared across omics) and individual structure (specific to each dataset). Produces joint structure scores, per-block individual scores, and a variance decomposition chart.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Components | 2 | Number of components for the full decomposition |
| Joint components | 2 | Rank of the joint structure |
| Individual components | 2 | Rank of each block's individual structure |

### MOFA+ — Multi-Omics Factor Analysis

Decomposes all datasets into shared latent factors explaining co-variation across omics layers. Each factor captures a coordinated pattern — e.g. a factor driven by both metabolite levels and gene expression.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Factors | 10 | Number of latent factors to learn |

Results include factor scores per sample, feature weights per factor per dataset, and variance explained per factor.

### SNF — Similarity Network Fusion

Builds a per-dataset sample similarity network and fuses them iteratively. Assigns samples to clusters based on the fused network. Useful when datasets are very heterogeneous.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Clusters | 3 | Number of clusters in the fused network |
| K neighbours | 20 | Nearest neighbours used for each sample graph |

---

## Combined Network

Builds a cross-dataset correlation network from the top N most variable features across all datasets.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Top features | 20 | Number of highest-variance features to include |
| Threshold | 0.6 | Minimum \|r\| for an edge to appear in the network |

Two views are produced:

- **Correlation heatmap** — all pairwise correlations among selected features
- **Network graph** — spring-layout graph; blue edges = positive correlation, red = negative; nodes coloured and shaped by source dataset

A table of significant edges (sorted by \|r\|) is available below each plot.

---

## Plot options

All score plots (PCA, MCIA joint scores, JIVE joint scores, MOFA factor scores) support post-hoc styling via the **Plot Options** panel:

| Option | Description |
|--------|-------------|
| Marker size | Adjust point size across all scatter plots |
| Color by | Colour samples by any metadata column from any linked dataset |
| Shape by | Use marker shape to encode a second metadata variable |

Metadata is merged from all selected datasets — overlapping columns with identical values are deduplicated; conflicting values appear as separate suffixed columns (e.g. `condition__dataset1`, `condition__dataset2`).

---

## Saving and reloading analyses

After a run completes, click **Save** in the results header to store the analysis with an optional name. Saved (and recent unsaved) analyses appear in the **Load recent analysis** dropdown above each section's results panel, marked with ★ if saved.

---

## Example: CCLE paired analysis

The built-in CCLE demo includes bulk metabolomics and pseudo-bulk scRNA-seq for 188 matched cancer cell lines. To explore:

1. Open the CCLE project from the home page
2. Go to Multi-Omics
3. Both datasets are linked by cell line name automatically
4. Run MOFA to identify transcriptomic–metabolomic co-variation factors
5. Use **Color by → primary_disease** in Plot Options to see cancer-type structure

## Example: microbiome–metabolome studies

All 14 paired microbiome–metabolome demo studies have microbiome and metabolomics datasets under the same project, linked by sample ID. Use the Combined Network view to find metabolites that associate with specific bacterial genera.
