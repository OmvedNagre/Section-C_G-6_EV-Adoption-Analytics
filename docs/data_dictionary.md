# Data Dictionary: Electric Vehicle Population Analysis

## How To Use This File
This file documents the transformation of raw registration data into an analytical dataset. It tracks original fields, cleaning steps, and the logic behind our custom-engineered KPIs.

## Dataset Summary

| Item | Details |
|------|---------|
| Dataset name | Electric Vehicle Population Data |
| Source | Washington State Department of Licensing (via Data.gov) |
| Raw file name | Electric_Vehicle_Population_Data.csv |
| Last updated | April 2026 |
| Granularity | One row per unique electric vehicle registration |

## Column Definitions

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|-------------|-----------|-------------|---------------|---------|----------------|
| VIN (1-10) | String | First 10 characters of the Vehicle Identification Number | 5YJ3E1EB6N | Tracking | None |
| County | String | County of vehicle registration in Washington | King | EDA / Tableau | Filled NaNs with 'Unknown' |
| City | String | City of vehicle registration | Seattle | EDA / Tableau | Filled NaNs with 'Unknown' |
| Model Year | Integer | The year the vehicle was manufactured | 2022 | KPI / EDA | None |
| Make | String | The vehicle manufacturer | TESLA | KPI / EDA | Used for Brand Segmentation |
| Electric Range | Float | Distance the vehicle can travel on a full charge | 215.0 | KPI / Stats | Treated 0 as NaN to avoid skewing averages |
| CAFV Eligibility | String | Clean Alternative Fuel Vehicle program status | Clean Fuel Eligible | KPI / EDA | Simplified into 'Eligibility_Status' |
| Electric Utility | String | Local utility provider(s) for the registration area | PUGET SOUND ENERGY | EDA | Used for Complexity Score calculation |
| Vehicle Location | String | POINT coordinates provided by the state | POINT (-122 47) | ETL | Parsed into Lat/Long columns |

## Derived Columns (Feature Engineering)

| Derived Column | Logic | Business Meaning |
|----------------|-------|------------------|
| Market_Segment | If Make in [Luxury List] then 'Luxury' else 'Mass Market' | Analyzes the socio-economic accessibility of EV adoption. |
| Utility_Complexity_Score | Count of distinct utility names in the Electric Utility string | Measures "Administrative Friction" and grid coordination difficulty. |
| Adoption_Wave | Binning Model Year: Pre-2015 (Early), 2016-2020 (Growth), 2021+ (Mass) | Visualizes the transition from early adopters to the general public. |
| Eligibility_Status | Mapping CAFV strings to 'Eligible', 'Not Eligible', or 'Unknown' | Provides clean, high-level filters for dashboard storytelling. |
| Latitude / Longitude | Regular Expression extraction from Vehicle Location string | Enables high-precision geospatial heatmaps in Tableau. |

## Data Quality Notes

**Range Data Gaps**: Approximately 64% of the dataset contains '0' for Electric Range. This indicates the range has not been researched/recorded by the state, rather than a literal zero-mile range.

**Geographic Precision**: Latitude and Longitude were extracted from POINT strings; records with corrupt or missing POINT data were dropped to maintain map integrity.

**Mixed Utility Zones**: Many areas are served by overlapping utility jurisdictions (marked by ||), which is a key focus of our infrastructure complexity analysis.

## Data Source Reference
Original dataset available at: https://catalog.data.gov/dataset/electric-vehicle-population-data