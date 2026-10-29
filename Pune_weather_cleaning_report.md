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

## Missing-value variants

The methods below are applied to eligible numeric measurements. Rainfall, wind
direction, sunshine, and snow are not imputed by these methods because their
measurement semantics make generic numeric filling inappropriate.

- **`weather_none.csv`**: leaves missing values empty.
- **`weather_forward_fill.csv`**: fills gaps of up to three consecutive rows
  from the preceding available value. Longer gaps and leading gaps stay empty.
- **`weather_interpolate.csv`**: linearly fills gaps of up to three consecutive
  rows when valid values bracket the gap. Unbracketed and longer gaps stay
  empty.
- **`weather_train_median.csv`**: fills eligible missing values with the
  per-source-file median calculated from that file's chronological training
  portion only.

## Verification

- The source summary covers all 116 CSV files and 6,445,130 data rows.
- The audit recorded 0 skipped timestamp rows and 0 duplicate timestamps.
- All four CSVs in the ZIP were checked to contain 6,445,130 data rows and 113
  columns each.
- The ZIP contains exactly the four plain `.csv` data files listed above.

## Interpretation and limitations

The imputed variants intentionally replace some missing measurements. Use the
missingness indicator columns to identify values that were originally missing.
The `none` variant is the reference when filling is not wanted. The dataset has
not been independently validated for physical plausibility, station
continuity, unit changes, or sensor changes; review those considerations and
the forecasting target before using it for modeling.
