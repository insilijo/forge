# Clustering

The Clustering module groups samples and features by similarity in feature space. Results can be coloured by any metadata variable to reveal structure in the data.

---

## Dimensionality reduction

High-dimensional omics data is first reduced to 2–3 dimensions for visualisation.

### PCA — Principal Component Analysis

Linear method. Projects samples onto axes of maximum variance. Fast and interpretable — the loadings show which features drive each component.

| Parameter | Default | Description |
|-----------|---------|-------------|
| Components | 2 | Number of principal components to compute and plot |

The score plot shows samples in PC space. The loading plot shows which features contribute most to each component. The scree plot shows variance explained per component.

### UMAP — Uniform Manifold Approximation and Projection

Non-linear method. Preserves local neighbourhood structure. Better at revealing clusters than PCA, especially in complex datasets. Slower and stochastic — results vary slightly between runs.

| Parameter | Default | Description |
|-----------|---------|-------------|
| n_neighbors | 15 | Controls local vs. global structure. Lower = more local detail |
| min_dist | 0.1 | Minimum distance between points in the embedding. Lower = tighter clusters |
| metric | euclidean | Distance metric in the original feature space |

UMAP results are not directly interpretable in terms of individual features — use PCA loadings for feature-level interpretation.

---

## Sample clustering

Groups samples into discrete clusters based on their feature profiles.

### Hierarchical clustering

Builds a tree (dendrogram) of samples by iteratively merging the most similar pairs.

| Parameter | Options | Description |
|-----------|---------|-------------|
| Linkage | ward, complete, average, single | How distance between clusters is computed. Ward minimises within-cluster variance and is usually preferred |
| Distance metric | euclidean, correlation, cosine | How similarity between samples is measured |

Results are shown as a dendrogram with an optional heatmap of feature values.

### k-means

Partitions samples into k clusters by minimising within-cluster sum of squares. Requires specifying k in advance.

| Parameter | Default | Description |
|-----------|---------|-------------|
| k | 3 | Number of clusters |
| Init | k-means++ | Initialisation method. k-means++ reduces sensitivity to starting conditions |
| Iterations | 10 | Number of random restarts; best result is kept |

Use the elbow plot (inertia vs. k) to help choose k.

---

## Feature clustering

Groups features by co-expression or co-abundance patterns across samples. Useful for identifying metabolite modules or gene programmes.

Run from the Clustering module by selecting **Features** instead of **Samples**. The same hierarchical and k-means methods are available.

Clustered features can be exported as a list for downstream enrichment analysis.

---

## Heatmap

The heatmap shows samples (columns) and features (rows) with colour representing scaled abundance. Both axes can be clustered independently.

| Option | Description |
|--------|-------------|
| Row clustering | Cluster features by similarity across samples |
| Column clustering | Cluster samples by similarity across features |
| Colour scale | Diverging (for centred data) or sequential |
| Annotation bar | Colour samples by a metadata variable |

By default, only the top 50 most variable features are shown. Increase this in the settings or filter to a feature set of interest.

---

## Colouring by metadata

All dimensionality reduction plots and heatmap annotation bars can be coloured by any column in your sample metadata. Use the **Colour by** dropdown in the plot toolbar. Categorical variables produce discrete colour palettes; continuous variables produce a gradient.

Patterns in the embedding that align with metadata variables (e.g. disease group, batch, age) indicate that those variables explain variance in the data.
