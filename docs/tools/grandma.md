# GrAndMA

**Graph And Metabolomic Analysis** — the web platform for multi-omics data analysis.

GrAndMA is the main user-facing tool in the insilijo ecosystem. It provides a browser-based interface for uploading, preprocessing, and analysing omics datasets, with no programming required.

## What it does

- Upload and manage metabolomics, transcriptomics, microbiome, proteomics, and other tabular omics datasets
- Preprocess data with configurable normalisation, imputation, log transformation, batch correction, and scaling
- Statistical analysis: differential abundance, correlation, longitudinal models
- Cluster samples and features with PCA, UMAP, hierarchical clustering, and k-means
- Build and explore metabolite networks with GIZMO-backed knowledge graph annotation
- Integrate multiple omics datasets (MCIA, JIVE, MOFA+, SNF, PCA/PLS-DA concat, correlation network); post-hoc color/shape by any metadata column; save and reload analyses
- Biomarker discovery: Discomarker (elastic net + SHAP + consensus clustering) and MetaSurv (Cox PH, KM, competing risk)
- Drug discovery pipeline: Ducky (target ranking from GIZMO disease scoring) → Chips (fragment search, ADMET triage, candidate ranking)
- Untargeted metabolomics: mzML upload, spectral annotation, feature extraction via Oredigger
- GeMMA integration: concordance analysis and active subnetwork detection for paired microbiome–metabolome studies
- 14 built-in paired microbiome–metabolome demo datasets plus a CCLE multi-omics demo

## Source

[github.com/insilijo/forge](https://github.com/insilijo/forge)

## Reporting issues

[Open an issue](https://github.com/insilijo/forge/issues/new/choose) on the forge issue tracker.
