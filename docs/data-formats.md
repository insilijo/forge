# Data Formats

## File types

GrAndMA accepts **CSV** (comma-separated) files. TSV (tab-separated) is not currently supported — convert using Excel or `pandas.read_csv(...).to_csv(...)` before uploading.

## The matrix file

The matrix is the core data file. It must be in **samples × features** orientation (one row per sample, one column per feature) unless you specify otherwise during configuration.

```
SampleID,glucose,lactate,citrate,alanine
S001,10.4,7.5,12.6,11.5
S002,10.8,8.1,11.4,11.7
S003,9.9,6.8,13.1,10.2
```

- The first row must be a header.
- One column must contain unique sample identifiers. This can be the row index (specify `__axis__` as the sample ID column) or a named column.
- All other columns are treated as features and must be numeric.

## The sample metadata file

One row per sample. The sample ID column must match the matrix.

```
SampleID,Group,TimePoint,Age,Sex
S001,Case,1,45,F
S002,Case,2,45,F
S003,Control,1,38,M
```

- Columns can be categorical (Group, TimePoint, Treatment) or continuous (Age, BMI, Score).
- Categorical columns are used for grouping in plots and differential abundance tests.
- Continuous columns can be used as covariates in regression models.
- A **batch column** can be included for batch correction.

## The feature metadata file

One row per feature. Feature IDs must match the column headers in the matrix.

```
feature_id,hmdb_id,pathway,annotation
glucose,HMDB0000122,Glycolysis,Glucose
lactate,HMDB0000190,Glycolysis,Lactic acid
citrate,HMDB0000094,TCA cycle,Citric acid
```

Feature metadata is optional but enriches network analysis and pathway mapping.

## Orientations

If your data has features as rows and samples as columns (e.g. MetaPhlAn output, some expression matrices), select **features × samples** orientation during configuration. GrAndMA will transpose it automatically.

## Missing values

Missing values should be represented as empty cells or `NA`. Do not use `0` to represent a missing measurement — use actual zeros only when a feature was genuinely not detected at a level above zero.

## Common pitfalls

| Problem | Fix |
|---------|-----|
| First column unnamed in Excel export | Add a column header (e.g. `SampleID`) or use `index_col=0` when exporting from pandas |
| Extra blank rows at end of file | Remove in Excel or with `dropna(how='all')` before export |
| Mixed numeric and text in a feature column | Check for stray characters (%, units, notes) in the matrix |
| Sample IDs don't match between matrix and metadata | Check for trailing spaces, case differences, or different ID formats |
| BOM character at start of file from Excel | Save as "CSV UTF-8 (comma delimited)" not "CSV (comma delimited)" |
