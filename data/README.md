# Data

This folder holds the project's data. The actual data files are **not tracked in Git** (listed in `.gitignore`)
due to their large size (Parquet ~715 MB) (original CSV ~2 GB).

```
data/
├── raw/         # Original monthly CSV files, 1/1/2024 - 31/3/2026 (input for notebook 01)
└── processed/   # Cleaned file: cyclistic_cleaned_data.parquet (output of notebook 01)
```

## Data source

The data is a public dataset from **Divvy / Cyclistic bike-share (Chicago)**,
published by Lyft Bikes and Scooters under the [Data License Agreement](https://divvybikes.com/data-license-agreement). It is real data that has been modified — e.g., personal information and company confidential data removed — leaving only ride data that can be used.

Download at: https://divvy-tripdata.s3.amazonaws.com/index.html (files are updated monthly)

## Data dictionary

See full column details at [`docs/data_dictionary.md`](../docs/data_dictionary.md)
