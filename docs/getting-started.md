# Getting Started

## What is GrAndMA?

GrAndMA is a web platform for analysing multi-omics data — metabolomics, transcriptomics, microbiome abundance, proteomics, lipidomics, and other tabular omics datasets. It is designed for researchers who want interactive analysis without writing code, as well as for those who prefer to automate workflows.

## Workflow overview

```
Upload data  →  Configure  →  Preprocess  →  Analyse  →  Explore results
```

### 1. Upload your data

GrAndMA accepts CSV files organised into three roles:

| File | Required | Description |
|------|----------|-------------|
| `matrix.csv` | Yes | The main data matrix — rows are samples, columns are features (metabolites, genes, taxa, etc.) |
| `sample_metadata.csv` | Recommended | One row per sample, columns are clinical or experimental variables (Group, TimePoint, Age, etc.) |
| `feature_metadata.csv` | Optional | One row per feature, columns are annotations (HMDB ID, pathway, etc.) |

See [Data Formats](data-formats.md) for full format details.

### 2. Configure your dataset

After upload, you will be asked to specify:

- **Data type** — Metabolomics, Transcriptomics, Microbiome, Proteomics, Lipidomics, or Generic
- **Orientation** — whether rows are samples or features
- **Sample ID column** — which column uniquely identifies each sample
- **Log transformation** — whether the data is already log-transformed

### 3. Preprocess

GrAndMA applies a standard preprocessing pipeline:

1. **Missingness filtering** — remove samples or features with too many missing values
2. **Imputation** — fill remaining missing values (KNN, minimum, or none)
3. **Normalisation** — probabilistic quotient, total ion count, or none
4. **Log transformation** — log2 or none
5. **Scaling** — autoscaling (z-score), Pareto, or none
6. **Batch correction** — ComBat or none (requires a batch column in sample metadata)

### 4. Analyse

Once preprocessed, the full analysis suite is available:

- **[Statistical analysis](stats.md)** — differential abundance tests, correlation, longitudinal models
- **[Clustering](clustering.md)** — PCA, UMAP, hierarchical clustering, k-means
- **[Network analysis](network.md)** — metabolite co-occurrence and pathway networks

### 5. Explore results

All results are interactive. Plots can be zoomed, filtered by metadata group, and downloaded as SVG or PNG. Tables can be sorted and filtered, and full result CSVs are available for download.

## Demo datasets

If you want to explore GrAndMA before uploading your own data, use the built-in demo datasets on the home page. See [Demo Datasets](demos.md) for a description of each study.
