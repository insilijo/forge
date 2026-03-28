# Statistical Analysis

The Stats module tests for differences in feature abundance between groups, over time, or in relation to continuous variables. All tests are run on the active preprocessed dataset.

---

## Differential abundance

Tests whether individual features differ significantly between two or more groups defined by a metadata column.

### Two-group tests

| Test | When to use |
|------|-------------|
| **Welch t-test** | Default. Parametric. Assumes approximate normality. Robust to unequal variances |
| **Wilcoxon rank-sum** | Non-parametric alternative. Use when normality is questionable or sample sizes are small |

### Multi-group tests

| Test | When to use |
|------|-------------|
| **One-way ANOVA** | Parametric. Tests whether any group mean differs from the others |
| **Kruskal–Wallis** | Non-parametric alternative to ANOVA |

### Multiple testing correction

All p-values are corrected for multiple comparisons across features.

| Method | Description |
|--------|-------------|
| **Benjamini–Hochberg (FDR)** | Default. Controls the expected proportion of false discoveries. Less conservative than Bonferroni |
| **Bonferroni** | Controls family-wise error rate. More conservative; appropriate when any false positive is unacceptable |
| **None** | No correction. Use only for exploratory analysis |

Results are shown as a volcano plot (effect size vs. –log10 adjusted p-value) and a sortable table. Significant features (FDR < 0.05 by default) are highlighted.

---

## Correlation analysis

Measures pairwise associations between features, or between features and a continuous metadata variable.

| Method | When to use |
|--------|-------------|
| **Pearson** | Linear correlation. Assumes normality |
| **Spearman** | Rank-based. More robust to outliers and non-linearity |

**Feature–feature correlation** produces a correlation matrix heatmap with optional hierarchical clustering of features.

**Feature–metadata correlation** tests each feature against a continuous variable (e.g. BMI, age, clinical score) and produces a ranked list with effect sizes and adjusted p-values.

---

## Longitudinal analysis

For datasets with repeated measurements from the same subjects over time. Requires a **subject ID column** and a **time column** in sample metadata.

| Model | When to use |
|-------|-------------|
| **Linear mixed-effects (LME)** | Models time as a fixed effect with a random intercept per subject. Handles unbalanced designs and missing time points |
| **Repeated-measures ANOVA** | Simpler parametric option. Requires complete, balanced data |

You can include additional fixed-effect covariates (e.g. group, age) in the model formula.

Results include: per-feature time effect p-values, estimated trajectories per group, and an interactive time-series plot for selected features.

---

## Enrichment analysis

Tests whether a set of significantly changed features is enriched in known biological pathways or functional categories.

### Over-representation analysis (ORA)

Takes your list of significant features (from differential abundance) and tests for over-representation in pathways using a Fisher exact test.

### Gene Set Enrichment Analysis (GSEA)

Ranks all features by their test statistic (e.g. fold change × –log10 p) and tests whether pathway members are concentrated at the top or bottom of the ranked list.

Pathway annotations are sourced from GIZMO (MetaNetX, Reactome, KEGG via ChEBI cross-references).

---

## Interpreting results

**Volcano plot** — features in the top-right quadrant are high in the comparison group relative to reference; top-left are lower. The horizontal dashed line marks the significance threshold.

**Effect size** — reported as log2 fold change (for two-group comparisons) or eta-squared (for ANOVA). Effect size and statistical significance are independent: a feature can be highly significant but biologically small, or large but non-significant in small samples.

**FDR threshold** — the default significance cutoff is FDR < 0.05. You can adjust this in the results filter. For exploratory work, FDR < 0.20 is sometimes used to cast a wider net.
