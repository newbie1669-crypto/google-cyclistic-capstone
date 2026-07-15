# Data Dictionary — `cyclistic_cleaned_data.parquet`

The analysis-ready data file contains **11,774,329 rows** and **21 columns**, covering trips from **2024 to early 2026**.

| # | column | data type | description |
|---|---------|-----------|----------|
| 1 | `ride_id` | string | Trip ID (unique identifier) |
| 2 | `rideable_type` | string | Bike type, e.g., `electric_bike`, `classic_bike` |
| 3 | `started_at` | timestamp | Trip start date and time |
| 4 | `ended_at` | timestamp | Trip end date and time |
| 5 | `day_of_week` | int | Day of week (0 = Monday … 6 = Sunday) |
| 6 | `holiday_name` | string | Name of the Illinois state holiday, or `None` for a regular day |
| 7 | `season` | string | Astronomical season (`Spring`, `Summer`, `Fall`, `Winter`) |
| 8 | `month` | int | Month (1–12) |
| 9 | `year` | int | Year |
| 10 | `start_station_name` | string | Start station name (`On-street` = no station, picked up/dropped off on the street via GPS) |
| 11 | `start_station_id` | string | Start station ID (`STREET` = no station) |
| 12 | `end_station_name` | string | End station name |
| 13 | `end_station_id` | string | End station ID |
| 14 | `start_lat` | double | Start latitude |
| 15 | `start_lng` | double | Start longitude |
| 16 | `end_lat` | double | End latitude |
| 17 | `end_lng` | double | End longitude |
| 18 | `member_casual` | string | User type: `member` (annual member) or `casual` (casual rider) |
| 19 | `duration` | duration | Trip duration (timedelta) |
| 20 | `duration_min` | double | Trip duration (minutes) |
| 21 | `duration_hour` | double | Trip duration (hours) |

## What was done to the data (Data cleaning notes)

- Removed rows with duplicate `ride_id`
- Removed trips where `ended_at < started_at` (negative duration — implies traveling back in time)
- Removed trips shorter than 1 minute (unlikely to be real usage — probably from activities like system testing, sudden cancellations, or errors)
- Filled missing station values with `On-street` / `STREET` (electric bikes parked on the street, since bikes can be parked on the street thanks to GPS and IoT systems)
- Imputed missing lat/lng coordinates with the average for that station (mean imputation), for cases where lat/lng is missing but the station is still known
- Where both station and coordinates were missing, the row was dropped since the data couldn't be used
- Added features useful for analysis: `day_of_week`, `month`, `holiday_name`, `season`
