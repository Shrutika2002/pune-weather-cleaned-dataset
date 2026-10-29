# Pune Weather Dataset Cleaning Report


## Deliverable


The model-ready dataset is `Pune_weather_training_data.zip`. It contains four
plain CSV files and no source files, summary file, scripts, or documentation:


- `weather_none.csv`
- `weather_forward_fill.csv`
- `weather_interpolate.csv`
- `weather_train_median.csv`


Each file contains 6,445,130 data rows and 113 columns. These are alternative
missing-value treatments of the same cleaned dataset; do not concatenate them.


## Source data and preservation


- Read 116 CSV files recursively from the original `Pune All data` folder.
- Left the original station CSV files unchanged.
- Kept a station and relative source-file identifier on each output row.
- Parsed and normalized source date/time into the `timestamp_utc` column.
- Sorted records by timestamp within each source file.
- Used an 80% chronological training split and 20% test split within each
  source file; the split is recorded in the `split` column.


## Cleaning and quality handling


- Standardized column names to consistent lowercase, underscore-separated names
  and mapped recognized source headers to canonical measurement names.
- Preserved distinct repeated column names with numbered suffixes and retained
  source quality-control flag columns.
- Treated blank cells and recognized missing-value tokens as missing values.
- Parsed finite numeric measurements as numbers. Non-numeric or non-finite
  measurement values are treated as missing in the cleaned tables.
- Normalized single-digit hours in time values before parsing timestamps.
- Rows with unparsable timestamps would be excluded; the source audit recorded
  **0** such rows.
- Duplicate timestamps within a source file would keep the last record; the
  source audit recorded **0** duplicate timestamps.
- A `Faulty` quality flag invalidates its paired measurement. Other quality
  flags are retained as audit information and are not assumed to mean a
  measurement is invalid.
- Added a `<measurement>_was_missing` indicator for each measurement, recording
  whether it was missing before imputation.
