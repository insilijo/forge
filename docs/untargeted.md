# Untargeted Metabolomics

GrAndMA's untargeted pipeline handles batch mzML upload, indexing, feature extraction, and spectral annotation across a whole project. Raw-spectrum features such as CCS extraction, chimeric deconvolution, and neutral-loss networks run inline as part of feature extraction.

---

## Untargeted Pipeline (project-level)

The project-level untargeted pipeline handles batch mzML upload, indexing, and spectral annotation across a whole project.

### Steps

1. **Upload mzML files** — attach one or more mzML files to a project; each file is immediately indexed (scan counts, m/z range, RT range, ion mobility flag)
2. **Run pipeline** — triggers feature extraction and spectral annotation across all uploaded files
3. **Review results** — annotated feature table is available for download and can be imported as a new dataset for downstream analysis

### Spectral annotation

Annotation uses a database-backed spectral library search. Features are matched by precursor m/z and MS2 spectrum similarity. Match confidence is reported as a score (0–1) alongside the database accession, compound name, and InChIKey.

### Importing results as a dataset

Once the pipeline completes, the annotated feature table can be promoted to a first-class GrAndMA dataset and taken through the standard preprocessing → analysis workflow.
