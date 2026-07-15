# **Cyclistic Bike Share - Google Data Analytics Capstone Project**

![bike](docs/image/victor_victor.png)

This project is the capstone for the [**`Google Data Analytics Professional Certificate`**](https://www.coursera.org/professional-certificates/google-data-analytics), which I completed a while back, starting in 2024. I lost my original project files, so I redid it as this one.

The task is to analyze Cyclistic's bike-share usage data to find behavioral differences
between **`annual members`** and **`casual riders`**,
in order to come up with a marketing plan to convert casual riders into members.

---

## **Background**

**`Cyclistic`** is a bike-share company in Chicago. The marketing and finance teams believe the key to growth
is converting **`casual riders`** into **`annual members`**. This project analyzes real trip data
of over **11.7 million records (2024–2026)** to understand how the two user groups use the service differently.

---

## **Business Task**

The task requires answering these key guiding questions:

- **" How do annual members and casual riders use Cyclistic bikes differently? "**

- **" Why would casual riders buy a Cyclistic annual membership? "**

- **" How can Cyclistic use digital media to influence casual riders to become members? "**

The analysis results will help answer these questions and support marketing strategy recommendations to convert casual riders into members.

## **Data Source**

The data is a public dataset from **`Divvy bike-share`** (Chicago), published by Lyft Bikes and Scooters under a [`Divvy Data License Agreement`](https://divvybikes.com/data-license-agreement). It is real data that has been modified — e.g., personal information and company confidential data removed — leaving only ride data that can be used.

**Download** at [`divvy-tripdata.s3.amazonaws.com`](https://divvy-tripdata.s3.amazonaws.com/index.html). files are updated monthly; there's a huge amount of data, great for further practice or projects as well.

Data and column details: [`docs/data_dictionary.md`](docs/data_dictionary.md)

## **Project Structure**

```plain text
google-cyclistic-capstone/
├── data/                 
│   ├── raw/              # Original monthly CSV files (not tracked in Git)
│   ├── processed/        # Cleaned files as parquet (not tracked in Git - due to 2GB+ size)
│   └── README.md         # Data source info
├── notebooks/
│   ├── 01_data_preparation.ipynb           # Merge files + data cleaning + add features
│   └── 02_exploratory_data_analysis.ipynb  # Analysis + dataviz
├── reports/
│   └── cyclistic_capstone_report.pdf       # PDF report
├── docs/
│   └── data_dictionary.md                  # Data description
├── .gitignore
├── LICENSE
└── README.md
```

## **Workflow**

### **Data Preparation** - `01_data_preparation.ipynb`

- Merge multiple monthly CSV files into a single DataFrame
- Check and handle data quality:
  - Remove duplicate `ride_id`
  - Remove trips with negative duration (`ended_at < started_at`)
  - Remove trips shorter than 1 minute (unlikely to be real usage)
  - Handle missing station values (`On-street`) and impute missing coordinates with the station's average
- Add features useful for analysis: `day_of_week`, `month`, `season`, `holiday_name`,
  `duration_min`, `duration_hour`
- Save the result as a **Parquet** file (more efficient than CSV for large data)

### **Analysis** - `02_data_analysis.ipynb`

Compare behavior between **member** and **casual**:

- **Trip count & duration** — overview and year-on-year growth
- **Bike type** (`rideable_type`) preferred by each group
- **Day of week** — comparing weekdays vs. weekends
- **Month and season** — seasonal patterns
- **Hour of day** — commute hours vs. leisure hours
- **Popular stations** and **travel routes**

## **Report**

**Full analysis report with visuals and recommendations :** [`reports/`](reports/)

## **Tools**

| Category | Tool |
| --- | --- |
| Language | `Python` |
| Data wrangling | `pandas`, `numpy` |
| Data storing | `Parquet (pyarrow)` |
| Visualization | `matplotlib`, `seaborn`, `plotly` |

## 📄 License

**Repository** : [`LICENSE`](LICENSE)

**Data** : [`Divvy Data License Agreement`](https://divvybikes.com/data-license-agreement)
