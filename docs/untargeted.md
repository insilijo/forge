# Untargeted Metabolomics

GrAndMA supports untargeted metabolomics workflows through two complementary paths: the Oredigger app for processing raw mzML files within a dataset context, and the Untargeted pipeline for project-level mzML batch processing with spectral annotation.

---

## Oredigger (dataset-level)

Oredigger processes raw mzML files attached to a dataset and produces an enriched feature table and a neutral loss network.

### What it does

1. Reads MS2 spectra from each mzML file using a streaming parser
2. Deconvolves chimeric spectra
3. Annotates features with collision cross section (CCS) estimates where ion mobility data is present
4. Identifies neutral losses and constructs a neutral loss network (feature–feature edges keyed by Δm/z)
5. Writes an enriched feature Parquet and a neutral loss network TSV to `MEDIA_ROOT/oredigger/<run_id>/`

### Requirements

- A dataset with one or more `.mzML` files attached via the Upload step
- A completed preprocessing run is not required — Oredigger operates on raw spectra

### Output

| File | Contents |
|------|----------|
| `enriched_features.parquet` | Per-feature table with m/z, RT, CCS, precursor info, deconvolution flag |
| `nl_network.tsv` | Edge list: feature pairs linked by neutral loss Δm/z with loss identity |

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
