# Demo Datasets

GrAndMA includes a curated set of demo datasets that are available on the home page without uploading any files. These are real published datasets, pre-processed and ready to explore.

## Longitudinal Metabolomics

A synthetic longitudinal plasma metabolomics dataset. 30 subjects (Case/Control), 3 time points, 80 metabolites. Useful for exploring the longitudinal analysis and mixed-effects modelling features.

**Key metadata columns:** `SubjectID`, `TimePoint`, `Group`

---

## CCLE — Cancer Cell Line Encyclopedia

Two paired datasets from the [DepMap project](https://depmap.org) and the [Kinker et al. 2020](https://www.nature.com/articles/s41588-020-00726-6) pooled scRNA-seq study. Both datasets share cell line names as sample IDs, enabling multi-omics analysis.

### CCLE Bulk Metabolomics
188 cancer cell lines × 225 metabolites. Log-transformed LC-MS measurements from the CCLE metabolomics panel.

### CCLE scRNA-seq (Kinker et al. 2020)
188 cancer cell lines × 3,000 highly variable genes. Pseudo-bulk profiles aggregated from the pooled single-cell RNA-seq experiment (GSE157220).

**Key metadata columns:** `primary_disease`, `n_cells`, `ach_id`

---

## Microbiome–Metabolome Studies

14 paired gut microbiome and metabolomics datasets from the [Borenstein lab curated resource](https://github.com/borenstein-lab/microbiome-metabolome-curated-data). Each study includes a genus-level microbiome abundance matrix and a metabolomics matrix, with shared sample IDs for multi-omics integration.

| Study | n | Groups | Notes |
|-------|---|--------|-------|
| FRANZOSA_IBD_2019 | 220 | CD, UC, Control | IBD microbiome + metabolomics |
| YACHIDA_CRC_2019 | 347 | CRC stages, Control | Colorectal cancer |
| WANG_ESRD_2020 | 287 | ESRD, Control | End-stage renal disease |
| MARS_IBS_2020 | 444 | IBS subtypes | Longitudinal, includes flare data |
| KIM_ADENOMAS_2020 | 240 | Adenoma, Control | Pre-cancerous polyps |
| SINHA_CRC_2016 | 131 | CRC, Control | Colorectal cancer |
| ERAWIJANTARI_GASTRIC_CANCER_2020 | 96 | Gastrectomy, Healthy | Gastric cancer surgery |
| JACOBS_IBD_FAMILIES_2016 | 90 | IBD, Control | Family-based IBD study |
| KOSTIC_INFANTS_DIABETES_2015 | 103 | T1D, Control | Infant gut, type 1 diabetes |
| POYET_BIO_ML_2019 | 164 | Healthy | Biobank, healthy adults |
| WANDRO_PRETERMS_2018 | 75 | Preterm groups | NICU preterm infants |
| HE_INFANTS_MFGM_2019 | 277 | Diet groups | Infant formula trial |
| KANG_AUTISM_2017 | 44 | ASD, Control | Autism, small n |
| iHMP_IBDMDB_2019 | 382 | IBD, Control | Longitudinal iHMP, microbiome only |

Each study has two datasets under the same project — select between them using the dataset dropdown in the sidebar.
