# MAST30034 Applied Data Science — Project 1

## Airport Taxi Demand and Driver Positioning in New York City

This project investigates temporal patterns in Yellow Taxi demand around New York City's JFK and LaGuardia (LGA) airports and examines whether scheduled flight-arrival information improves the prediction of airport taxi demand.

The analysis combines NYC Yellow Taxi trip records with U.S. Bureau of Transportation Statistics (BTS) flight-arrival data. The project first constructs hourly airport-level taxi and flight datasets, explores the relationship between scheduled arrivals and taxi pickups, compares predictive models, and finally translates the selected model into operational driver-positioning recommendations.

The main analysis is implemented using **Python and PySpark**. Pandas and Matplotlib are used only after aggregation when the data are sufficiently small for local visualisation.

---

## 1. Research Question

The project addresses two related questions:

1. Does scheduled flight-arrival information improve the prediction of hourly Yellow Taxi pickup demand at JFK and LaGuardia Airport?
2. When does predicted airport taxi demand become sufficiently high and persistent to provide useful driver-positioning guidance?

The analysis focuses on JFK and LaGuardia Airport and uses a chronological train/test design so that future observations are not used to predict the past.

---

## 2. Repository Structure

The repository is organised as follows:

```text
Project_1/
│
├── README.md
├── .gitignore
│
├── notebooks/
│   ├── 01_taxi_data.ipynb
│   ├── 02_flight_data.ipynb
│   ├── 03_analysis.ipynb
│   ├── 04_modeling.ipynb
│   └── 05_recommendations.ipynb
│
├── data/
│   ├── raw/
│   │   ├── taxi/
│   │   └── flights/
│   │
│   └── processed/
│       ├── hourly_airport_taxi_demand.parquet/
│       ├── hourly_flight_arrivals.parquet/
│       └── final_gbt_predictions.parquet/
│
├── plots/
│   ├── hourly_arrivals_taxi_profiles.png
│   ├── lagged_correlations_by_airport.png
│   ├── weekday_weekend_demand_by_airport.png
│   ├── model_rmse_comparison.png
│   ├── predicted_demand_by_airport.png
│   ├── recommended_positioning_hours_by_airport.png
│   └── predicted_demand_heatmap_by_day_type.png
│
└── report/
```

Some generated files and large raw datasets may not be committed to Git. They can be reproduced by following the instructions below.

---

## 3. Software Requirements

The project was developed using:

- Python 3.11
- PySpark
- Apache Spark
- Java
- pandas
- NumPy
- Matplotlib
- Jupyter
- Git


# 4. Data Sources

Two main external data sources are used.

## 4.1 NYC Yellow Taxi Trip Data

Yellow Taxi trip records are obtained from the **NYC Taxi & Limousine Commission (TLC) Trip Record Data**.

The analysis uses Yellow Taxi data covering the study period used in the notebooks.

Download the required Yellow Taxi Parquet files from the NYC TLC Trip Record Data page and place them in:
https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page

The raw taxi files should remain in their original Parquet format.

`01_taxi_data.ipynb` reads these raw files, identifies airport pickup trips, performs the required cleaning and aggregation, and produces the hourly airport taxi-demand dataset used by the later notebooks.

---

## 4.2 BTS Flight Data — Manual Download Required


Before running `02_flight_data.ipynb`, the flight data must be downloaded manually from the **U.S. Bureau of Transportation Statistics (BTS)**.

Download the Marketing Carrier On-Time Performance (Beginning January 2018) pre-zipped files for January–June 2025 from the BTS TranStats download page, then place the downloaded ZIP files in the raw flight-data directory specified below.
https://www.transtats.bts.gov/DL_SelectFields.aspx?QO_fu146_anzr=&gnoyr_VQ=FGK&utm_source



Download the monthly data for:

- January 2025

- February 2025

- March 2025

- April 2025

- May 2025

- June 2025

> **Important:** The same fields must be selected for all six monthly downloads

> so that the files have an identical schema and can be processed consistently

> by `02_flight_data.ipynb`.

For **each month from January to June 2025**, select the following fields:

| BTS Category | Field Name | Required for this project |

|---|---|---|

| Time Period | `FlightDate` | Yes |

| Airline | `Reporting_Airline` | Yes |

| Origin/Destination | `Origin` | Yes |

| Origin/Destination | `Dest` | Yes |

| Arrival Performance | `CRSArrTime` | Yes |

| Arrival Performance | `ArrTime` | Yes |

| Arrival Performance | `ArrDelay` | Yes |

| Arrival Performance | `WheelsOn` | Yes |

| Cancellations/Diverted | `Cancelled` | Yes |

| Cancellations/Diverted | `Diverted` | Yes |

Use the following identical field selection for every monthly


BTS will provide the data as a `.zip` archive.

### Rename the downloaded BTS files

After downloading the six monthly BTS ZIP files, **rename each ZIP file according
to its corresponding month** using the naming convention:

    flight_arrivals_YYYY_MM.zip

For this project, the six files must therefore be renamed as follows:

| Month | Required filename |
|---|---|
| January 2025 | `flight_arrivals_2025_01.zip` |
| February 2025 | `flight_arrivals_2025_02.zip` |
| March 2025 | `flight_arrivals_2025_03.zip` |
| April 2025 | `flight_arrivals_2025_04.zip` |
| May 2025 | `flight_arrivals_2025_05.zip` |
| June 2025 | `flight_arrivals_2025_06.zip` |

**Do not extract the ZIP files unless instructed by the notebook.** Keep the
files in ZIP format and place all six renamed files in the raw flight-data
directory expected by `02_flight_data.ipynb`.

The final directory should contain:

    flight_arrivals_2025_01.zip
    flight_arrivals_2025_02.zip
    flight_arrivals_2025_03.zip
    flight_arrivals_2025_04.zip
    flight_arrivals_2025_05.zip
    flight_arrivals_2025_06.zip

The filenames are used to keep the monthly BTS data organised and to ensure
that the preprocessing workflow can identify the files consistently.


# 5. Reproducing the Analysis

## Important: Run the notebooks in order

The notebooks form a sequential pipeline. Later notebooks depend on files generated by earlier notebooks.

Run them in exactly the following order:

```text
01_taxi_data.ipynb
        ↓
02_flight_data.ipynb
        ↓
03_analysis.ipynb
        ↓
04_modeling.ipynb
        ↓
05_recommendations.ipynb
```

Running notebooks out of order may produce missing-file errors.

