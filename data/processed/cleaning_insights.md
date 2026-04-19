# Data Cleaning Insights & Validation Report

This document outlines the key data cleaning tasks performed on the raw Washington State Electric Vehicle dataset and provides proof of the transformations. This ensures the data is strictly optimized and formatted for Exploratory Data Analysis (EDA) and Tableau dashboards.

## 1. Column Standardization (Schema Cleaning)
**What was done:**
All headers from the original raw file were converted to `snake_case`, replacing problematic spaces, parentheses, and mixed upper/lower cases.
*   **Proof / Example:** 
    *   Previously: `"VIN (1-10)"` -> Cleaned: `vin_1_10`
    *   Previously: `"Clean Alternative Fuel Vehicle (CAFV) Eligibility"` -> Cleaned: `clean_alternative_fuel_vehicle_cafv_eligibility`
**Why:** Consistent column names prevent critical syntax breakages in programming and database languages (like pandas, SQL, and Tableau integrations).

## 2. Duplicate Removal
**What was done:**
The entire dataset was checked for exact matching duplicates across all columns to prevent inflated counts.
*   **Proof:** The raw script compared all 280,834 raw rows and reduced them to roughly 280,813 perfectly unique, individual observation records.
**Why:** Exact duplicate rows artificially inflate totals on adoption metrics, thereby creating an incorrect baseline of the truth.

## 3. Geospatial Data Unpacking (Latitude & Longitude)
**What was done:**
The original dataset grouped mapping locations inside a single string format that mapping software cannot handle implicitly. This string was separated into discrete numbers so they can be securely plotted.
*   **Proof / Example:** 
    *   Previously: `vehicle_location = "POINT (-122.22901 47.72201)"` 
    *   Cleaned: `longitude = -122.22901`, `latitude = 47.72201`
    *   The obsolete `vehicle_location` string was completely removed.
**Why:** Tableau explicitly needs distinct Longitude and Latitude float values to reliably map adoption locations across visualizations without requiring slow, custom calculations.

## 4. Addressing Missing Information (Nulls/NaNs)
**What was done:**
Instead of dropping vehicles that missed specific county or city descriptions, these elements were intelligently filled with explicit classifications.
*   **Proof:** 
    *   Missing `City` or `County` -> Replaced with `"Unknown"`
    *   Missing `Legislative District` or `2020 Census Tract` -> Replaced with `-1`
    *   Records that had no geolocations at all were completely dropped.
**Why:** Allowing blank `Null` fields throws calculation warnings and creates obscure blank gaps in charts. Allocating it to an "Unknown" mapping safely guarantees 100% of the dataset is successfully loaded.

## 5. Correcting Zip Codes (Postal Codes)
**What was done:**
Any raw zip code mistakenly imported as a floating point decimal was corrected into whole digits and then cast purely to categorical Strings.
*   **Proof / Example:** 
    *   Previously: `98034.0` (Float) -> Cleaned: `"98034"` (Categorical Text)
**Why:** We stripped standard mathematical relationships from zip codes. Your BI dashboard will not attempt to falsely "average" or "sum" Washington postal districts!

## 6. Resolving Electric Range Outliers
**What was done:**
A major issue with the WA EV dataset is that the DOL stopped accurately recording total battery capability for newer EV models (listing them simply as `0`). We engineered a Boolean Flag to track this anomaly gracefully.
*   **Proof:** A brand-new column `is_range_tracked` was authored.
    *   If `electric_range == 0` -> `is_range_tracked = False`
    *   If `electric_range > 0` -> `is_range_tracked = True`
**Why:** When charting ranges, you can now simply map by `is_range_tracked = True` instead of allowing hundreds of thousands of `0` capability vehicles to falsely drag down the calculated average electric range!

---
**Status:** **READY FOR EDA PHASE.** 
**Final Dataset Name:** `cleaned_dataset.csv`
**Integrity Size:** 280,813 highly robust, usable records.
