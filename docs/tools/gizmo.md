# GIZMO

**Graph-Integrated Zone of Metabolite Operations** — the metabolite knowledge graph that powers GrAndMA's network analysis.

GIZMO is the backend library GrAndMA uses for network construction, pathway annotation, and cross-database identifier mapping. Most GrAndMA users never interact with it directly — it runs underneath the Network Analysis and annotation features. This page is for developers, integrators, or curious users who want to understand how those features work.

## What it does

- Builds a unified metabolite reaction graph integrating MetaNetX, ChEBI, PubChem, HMDB, and Reactome
- Maps metabolite identifiers across databases
- Annotates metabolites with chemical structure (SMILES, InChI, formula, mass)
- Links metabolites to reactions, pathways, genes, and disease associations (Open Targets, MONDO, Orphanet)
- Powers network analysis and enrichment in GrAndMA

## Source

[github.com/insilijo/GIZMO](https://github.com/insilijo/GIZMO)

## Reporting issues

Use the [forge issue tracker](https://github.com/insilijo/forge/issues/new/choose) and specify GIZMO in your report.
