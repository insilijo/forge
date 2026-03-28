# Network Analysis

The Network module builds and explores metabolite networks backed by the GIZMO knowledge graph. Networks connect metabolites through shared reactions, pathways, structural similarity, or statistical co-abundance in your data.

---

## Network types

### Reaction network

Connects metabolites that participate in the same biochemical reactions. Edges represent enzymatic conversions. Sourced from MetaNetX (integrating KEGG, Reactome, BiGG, and others via GIZMO).

Use this to explore metabolic pathway context — which reactions connect your metabolites of interest, which enzymes are involved, and whether a set of changed metabolites clusters in a specific pathway.

### Co-abundance network

Connects metabolites that are statistically correlated in your dataset. Edges represent Pearson or Spearman correlation above a threshold (default |r| > 0.6, FDR < 0.05).

Use this to discover groups of co-regulated metabolites in your data, independent of prior pathway knowledge.

### Combined

Overlays reaction and co-abundance edges. Reaction edges are shown in blue; co-abundance edges in orange. Convergence between the two (metabolites connected by both) highlights high-confidence relationships.

---

## Building a network

1. Go to the Network module for your dataset
2. Select network type
3. Choose a seed — either **all significant features** from a prior Stats run, or a **manual selection** of metabolite names
4. Set the neighbourhood depth (1 = direct connections only, 2 = neighbours of neighbours)
5. Click **Build network**

Network construction queries the GIZMO graph and may take a few seconds for large seed sets.

---

## Node annotation

Each node (metabolite) is annotated with:

- **Name** — preferred name from ChEBI or MetaNetX
- **Formula** — molecular formula
- **HMDB / ChEBI / PubChem IDs** — cross-database identifiers
- **Pathways** — Reactome and MetaNetX pathway membership
- **Disease associations** — linked conditions from Open Targets and MONDO (where available)
- **Fold change / p-value** — from the most recent Stats run (if available)

Click any node to open its annotation panel.

---

## Layout and visualisation

Networks are rendered interactively. Use the toolbar to:

| Control | Action |
|---------|--------|
| Drag nodes | Reposition manually |
| Scroll | Zoom in/out |
| **Colour by** | Colour nodes by fold change, p-value, pathway membership, or a metadata variable |
| **Size by** | Scale node size by fold change magnitude or feature variance |
| **Filter** | Show only nodes above a significance threshold |
| **Download** | Export as SVG, PNG, or GraphML (for Cytoscape) |

---

## Pathway enrichment in the network

The network view includes an enrichment panel showing which pathways are over-represented among the nodes currently displayed. This updates dynamically as you filter or expand the network.

Enrichment is computed using the same ORA (Fisher exact test) method as the Stats module, with the displayed nodes as the foreground set and all features in the dataset as the background.

---

## Shared graph

GrAndMA maintains a shared metabolite knowledge graph built from GIZMO. This graph is pre-built and updated periodically. For most datasets, network queries run against this shared graph without requiring a separate build step.

If you are working with a dataset that contains non-standard metabolite identifiers (e.g. custom internal standards, vendor-specific names), GIZMO's identifier mapping may not cover all features. Unmapped features will appear as isolated nodes in the network.
