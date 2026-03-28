# FAQ

## Data upload

**What file formats are supported?**
CSV only. See [Data Formats](data-formats.md) for structure requirements.

**My upload fails — what should I check?**
The most common causes are: non-numeric values in the matrix (check for text, units, or `%` symbols), sample IDs that don't match between the matrix and metadata, and files exported from Excel with a BOM character. See [Data Formats — Common Pitfalls](data-formats.md#common-pitfalls).

**Can I upload multiple files for one dataset?**
Yes. Each dataset can have a matrix, sample metadata, and feature metadata file. Upload them as separate files and assign each a role during configuration.

**How large can my files be?**
There is no hard limit, but matrices larger than ~100,000 features may be slow during preprocessing. Consider filtering to the most variable features before upload if performance is a concern.

---

## Preprocessing

**Should I log-transform my data before uploading?**
If your data is already log-transformed (e.g. log2 normalised expression, log-scale LC-MS from DepMap), tick "already log-transformed" during configuration and GrAndMA will skip the log step. If your data is in raw intensity units, leave it unticked and select log2 during preprocessing.

**What imputation method should I use?**
For metabolomics with <20% missingness, KNN (k=5) works well. For data with structural missingness (features absent from entire groups), consider minimum-value imputation or none.

**Can I re-preprocess a dataset?**
Yes. Go to the preprocessing page for that dataset and run again with different settings. Previous runs are preserved and you can switch between them.

---

## Analysis

**What statistical test is used for differential abundance?**
By default, a Welch t-test (two groups) or one-way ANOVA (three or more groups) with Benjamini–Hochberg FDR correction. Wilcoxon rank-sum and Kruskal–Wallis non-parametric alternatives are also available.

**How do I analyse longitudinal data?**
Use the Stats module and select the mixed-effects or repeated-measures model. You will need a Subject ID column and a TimePoint column in your sample metadata.

**Can I analyse multiple datasets together?**
Yes, if they share sample IDs. Use the Multi-Omics module to combine datasets from the same project. See [Multi-Omics](multiomics.md).

---

## Results

**How do I download my results?**
Each analysis result has a download button for CSV export. Plots can be downloaded as SVG or PNG from the toolbar in the top-right of each figure.

**Results disappeared after I came back — what happened?**
Analysis results are linked to your account and persist between sessions. If you are using a demo account, results may be cleared periodically. Log in with a personal account to retain results.

---

## Issues and feedback

**I found a bug. Where do I report it?**
[Open an issue](https://github.com/insilijo/forge/issues/new?template=bug_report.md) in this repository with steps to reproduce.

**I have a feature request.**
[Open a feature request](https://github.com/insilijo/forge/issues/new?template=feature_request.md).

**I have a private or sensitive question.**
Use the contact form at [insilijo.github.io](https://insilijo.github.io).
