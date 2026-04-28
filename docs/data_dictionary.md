# Data Dictionary: Electric Vehicle Population Analysis

## How To Use This File
This file documents every column in the final analytical dataset `EV_Final_Ready_Tableau.csv`, including original fields from the DOL registry, all cleaning decisions applied during the ETL pipeline, and the logic behind each engineered feature. Use this as the authoritative reference when reconnecting the dataset in Tableau or reproducing any analysis.

---

## Dataset Summary

| Item | Details |
|------|---------|
| Dataset name | Electric Vehicle Population Data |
| Source | Washington State Department of Licensing (via Data.gov) |
| Raw file name | `Electric_Vehicle_Population_Data.csv` |
| Cleaned file name | `EV_Final_Ready_Tableau.csv` |
| Intermediate file | `EV_Population_Cleaned.csv` |
| Raw row count | 280,813 |
| Analytical row count | 101,275 (verified electric range only) |
| Total columns | 26 (17 original + 9 engineered) |
| Granularity | One row = one unique electric vehicle registration |
| Geographic scope | Washington State, USA |
| Last updated | April 2026 |

---

## Part 1 — Original Columns (17)

### Column 1 — VIN (1-10)
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | First 10 characters of the Vehicle Identification Number. Unique per vehicle. |
| **Format** | Alphanumeric, always 10 characters |
| **Example** | `5YJ3E1EC8L`, `1N4AZ0CP6D` |
| **Nulls** | None |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Row-level identifier — not visualised directly |

---

### Column 2 — County
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Washington State county where the vehicle is registered |
| **Top Values** | King (46,605), Snohomish (10,718), Pierce (8,590), Clark (6,814), Thurston (4,103) |
| **Unique Count** | 170 (includes non-standard entries; 39 standard WA counties) |
| **Nulls** | Present in raw data |
| **Cleaning** | Missing values filled with `'Unknown'` to prevent row loss during grouping |
| **Dashboard Use** | Geographic filter, choropleth map dimension, county bar chart |

---

### Column 3 — City
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | City of vehicle registration |
| **Top Values** | Seattle (16,230), Vancouver (4,254), Bellevue (3,827), Olympia (2,668), Redmond (2,645) |
| **Nulls** | Present in raw data |
| **Cleaning** | Missing values filled with `'Unknown'` |
| **Dashboard Use** | City-level drill-down filter, bubble map label |

---

### Column 4 — State
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Always `WA` — Washington State. Constant field across entire dataset. |
| **Unique Values** | `WA` only |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Static map context label |

---

### Column 5 — Postal Code
| Property | Detail |
|---|---|
| **Data Type** | String (after cleaning) |
| **Description** | 5-digit ZIP code of registration address |
| **Raw Type** | Float (e.g., `98034.0`) — pandas inferred from CSV |
| **Example** | `98034`, `98110`, `98902` |
| **Nulls** | Present in raw data |
| **Cleaning** | (1) Filled NaNs with `'Unknown'`. (2) Converted to string. (3) Stripped `.0` suffix — `98034.0` → `98034` |
| **Dashboard Use** | ZIP-level geographic mapping |

---

### Column 6 — Model Year
| Property | Detail |
|---|---|
| **Data Type** | Integer |
| **Description** | Year the vehicle was manufactured |
| **Range** | 1999 – 2026 |
| **Key Distribution** | 2018: 13,956 · 2020: 11,970 · 2019: 10,772 · 2024: 10,723 · 2017: 8,397 |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Time-series axis, trend line, adoption wave classification, year filter |

---

### Column 7 — Make
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Vehicle manufacturer / brand |
| **Unique Count** | 36 |
| **Top Values** | TESLA (24,182), CHEVROLET (9,848), TOYOTA (9,750), NISSAN (9,631), BMW (7,048) |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Brand filter, horizontal bar chart, market segment source field |

---

### Column 8 — Model
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Specific vehicle model name |
| **Sample Values** | MODEL 3, MODEL Y, LEAF, BOLT EV, PRIUS PRIME (PHEV), WRANGLER, PACIFICA, RAV4 PRIME |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Model-level filter, treemap label, top models table |

---

### Column 9 — Electric Vehicle Type
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Electrification technology category |
| **Exact Values** | `Battery Electric Vehicle (BEV)` — 45,583 (45%) · `Plug-in Hybrid Electric Vehicle (PHEV)` — 55,692 (55%) |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Primary technology split filter, donut chart, stacked bar colour dimension |

---

### Column 10 — Clean Alternative Fuel Vehicle (CAFV) Eligibility
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Full original eligibility text from DOL registry — verbose and inconsistently formatted |
| **Sample Values** | `Clean Alternative Fuel Vehicle Eligible` · `Not eligible due to low battery range` · `Eligibility unknown as battery range has not been researched` |
| **Cleaning** | No changes applied to this column. Use `Eligibility_Status` (Column 24) for dashboard filtering. |
| **Dashboard Use** | Source field only — replaced by `Eligibility_Status` for visualisation |

---

### Column 11 — Electric Range
| Property | Detail |
|---|---|
| **Data Type** | Float |
| **Description** | EPA-rated electric-only driving range in miles — primary analysis column |
| **Range (valid)** | 1.0 – 337.0 miles |
| **Nulls after cleaning** | Present — original zeros replaced with `NaN` |
| **Cleaning** | All `0` values replaced with `np.nan`. A `0` in the DOL registry means "range not researched" — not a literal zero-mile vehicle. Including zeros would deflate averages. Original values preserved in `Electric Range Raw`. |
| **Impact** | 179,538 rows had `Electric Range = 0` → converted to `NaN` |
| **Dashboard Use** | Average range KPI, range evolution line chart, scatter plot axis |

---

### Column 12 — Legislative District
| Property | Detail |
|---|---|
| **Data Type** | Float |
| **Description** | Washington State legislative district number |
| **Range** | 1.0 – 49.0 |
| **Nulls** | Present in raw data |
| **Cleaning** | Missing values filled with `'Unknown'` |
| **Dashboard Use** | Policy analysis, district-level filter |

---

### Column 13 — DOL Vehicle ID
| Property | Detail |
|---|---|
| **Data Type** | Integer |
| **Description** | Washington State Department of Licensing unique vehicle identifier |
| **Format** | 6–9 digit integer e.g. `154635729` |
| **Cleaning** | No changes applied |
| **Dashboard Use** | `COUNT(DOL Vehicle ID)` used as the primary registration count measure across all KPIs |

---

### Column 14 — Vehicle Location
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Geographic coordinates in WKT POINT format |
| **Format** | `POINT (longitude latitude)` e.g. `POINT (-122.22901 47.72201)` |
| **Nulls** | Present in raw data |
| **Cleaning** | (1) NaNs filled with `'Unknown'`. (2) Parsed using regex `extract_coords` helper function into `Latitude` and `Longitude` columns. (3) Rows where coordinates could not be extracted were dropped. |
| **Dashboard Use** | Source field — use `Latitude` / `Longitude` (Columns 21–22) for all maps |

---

### Column 15 — Electric Utility
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Description** | Electric utility company or companies serving the registration area. Multiple utilities separated by `\|\|`. |
| **Sample Values** | `PUGET SOUND ENERGY INC` · `CITY OF SEATTLE - (WA)\|CITY OF TACOMA - (WA)` · `PACIFICORP` |
| **Cleaning** | No changes applied to this column. Used as source for `Utility_Complexity_Score`. |
| **Dashboard Use** | Source field for complexity score; utility provider filter |

---

### Column 16 — 2020 Census Tract
| Property | Detail |
|---|---|
| **Data Type** | Float |
| **Description** | 11-digit US Census Bureau tract identifier |
| **Example** | `53033022203.0`, `53035090902.0` |
| **Cleaning** | No changes applied |
| **Dashboard Use** | Census-level geographic analysis |

---

### Column 17 — Electric Range Raw
| Property | Detail |
|---|---|
| **Data Type** | Float |
| **Description** | Original electric range value — exact copy of `Electric Range` made **before** zero → NaN treatment |
| **Includes** | All values including zeros (zeros = "range not researched") |
| **Cleaning** | Created as: `df['Electric Range Raw'] = df['Electric Range'].copy()` prior to NaN substitution |
| **Purpose** | Used for population count analyses where `0` should be counted as a record. Do **not** use for average range KPIs. |
| **Dashboard Use** | `Range_Category` binning source; population volume counts |

---

## Part 2 — Engineered Columns (9)

### Column 18 — Vehicle Age
| Property | Detail |
|---|---|
| **Data Type** | Integer |
| **Logic** | `Vehicle Age = 2026 − Model Year` |
| **Range** | 0 (2026 model) to 27 (1999 model) |
| **Example** | 2018 model → Age 8; 2013 model → Age 13 |
| **Business Meaning** | Fleet aging analysis; used in age-vs-range correlation (r = −0.61) |
| **Dashboard Use** | Age vs Range scatter plot, fleet age distribution |

---

### Column 19 — CAFV_Status
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Logic** | If `CAFV Eligibility` contains `'Eligible'` → `'Eligible'`; else → `'Unknown/Not Eligible'` |
| **Exact Values** | `Eligible` (76,897) · `Unknown/Not Eligible` (24,074) |
| **Business Meaning** | Simplified 2-tier eligibility label for KPI cards and quick filtering |
| **Dashboard Use** | Eligibility KPI card, dashboard filter |

---

### Column 20 — Range_Category
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Source** | `Electric Range Raw` |
| **Bins & Labels** | `[-1, 0]` → `Unknown/Not Researched` · `(0, 100]` → `Urban Commuter` · `(100, 250]` → `Regional Traveler` · `(250, 1000]` → `Long Range` |
| **Distribution** | Urban Commuter: 63,848 (63%) · Regional Traveler: 27,529 (27%) · Long Range: 9,594 (9%) |
| **Business Meaning** | Human-readable range tier used for segment comparison and filtering |
| **Dashboard Use** | Range capability evolution stacked area chart, range segment bar chart |

---

### Column 21 — Latitude
| Property | Detail |
|---|---|
| **Data Type** | Float |
| **Logic** | Regex extracted from `Vehicle Location` POINT string: second numeric value |
| **Range (WA State)** | ~45.5 to ~49.0 |
| **Precision** | Up to 5 decimal places |
| **Example** | `47.72201`, `46.55029` |
| **Cleaning** | Rows where extraction failed (corrupt/missing POINT) were dropped |
| **Dashboard Use** | Map latitude field — assign Geographic Role: Latitude in Tableau |

---

### Column 22 — Longitude
| Property | Detail |
|---|---|
| **Data Type** | Float |
| **Logic** | Regex extracted from `Vehicle Location` POINT string: first numeric value |
| **Range (WA State)** | ~−124.8 to ~−116.9 |
| **Precision** | Up to 5 decimal places |
| **Example** | `−122.22901`, `−120.71847` |
| **Cleaning** | Rows where extraction failed were dropped |
| **Dashboard Use** | Map longitude field — assign Geographic Role: Longitude in Tableau |

---

### Column 23 — Market_Segment
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Logic** | If `Make` is in luxury brand list → `'Luxury'`; else → `'Mass Market'` |
| **Luxury Brands** | TESLA, PORSCHE, LUCID, RIVIAN, AUDI, BMW, MERCEDES-BENZ |
| **Mass Market Brands** | CHEVROLET, NISSAN, TOYOTA, FORD, KIA, HYUNDAI, JEEP, CHRYSLER, MITSUBISHI, HONDA, VOLKSWAGEN, FIAT, MAZDA, and others |
| **Distribution** | Luxury: 36,376 (36%) · Mass Market: 64,595 (64%) |
| **Business Meaning** | Evaluates socio-economic accessibility of EV adoption. Luxury-heavy adoption signals income-skewed uptake and highlights the need for mass-market incentive programs. |
| **Dashboard Use** | Adoption wave stacked bar colour, market accessibility analysis, KPI card |

---

### Column 24 — Eligibility_Status
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Logic** | 3-tier clean mapping from `CAFV Eligibility` raw text |
| **Mapping** | Contains `'Eligible'` → `'Eligible'` · Contains `'not eligible'` → `'Not Eligible'` · Contains `'unknown'` → `'Unknown'` |
| **Exact Values** | `Eligible` (76,897 — 76%) · `Not Eligible` (24,074 — 24%) |
| **Business Meaning** | Traffic-light eligibility filter for dashboard storytelling. Directly measures the reach and effectiveness of the CAFV incentive program. |
| **Dashboard Use** | CAFV donut chart, eligibility KPI card, primary dashboard filter |

---

### Column 25 — Adoption_Wave
| Property | Detail |
|---|---|
| **Data Type** | String |
| **Logic** | Binned from `Model Year` into three eras |
| **Bins** | `Model Year ≤ 2015` → `'Early'` · `2016 ≤ Model Year ≤ 2020` → `'Growth'` · `Model Year > 2020` → `'Mass Adoption'` |
| **Distribution** | Early: 13,436 (13%) · Growth: 50,005 (49%) · Mass Adoption: 37,530 (37%) |
| **Business Meaning** | Visualises the velocity of the EV transition across three distinct market phases: pioneering, expansion, and mainstream. |
| **Dashboard Use** | Adoption waves stacked bar, area chart colour dimension, trend analysis |

---

### Column 26 — Utility_Complexity_Score
| Property | Detail |
|---|---|
| **Data Type** | Integer |
| **Logic** | `len(Electric Utility.split('\|'))` — count of `\|`-separated utility jurisdiction segments |
| **Range** | 1 – 3 |
| **Score Meaning** | `1` = single utility · `2` = two jurisdictions · `3` = three or more jurisdictions |
| **Example** | `PUGET SOUND ENERGY INC` → Score 1 · `PUGET SOUND ENERGY INC\|\|CITY OF TACOMA` → Score 2 |
| **Business Meaning** | Proxy for Administrative Friction. Higher score = more agencies must coordinate for grid upgrades. Rural counties (Asotin, Mason, Jefferson) score 3.0 despite low EV density — signalling high future coordination cost. |
| **Dashboard Use** | Infrastructure Friction horizontal bar chart (Dashboard 2) |

---

## Part 3 — Data Quality Notes

### 3.1 The "Zero Range" Problem
Approximately **64% of the raw dataset** (179,538 out of 280,813 rows) contains `0` for `Electric Range`. This does **not** represent a zero-mile vehicle — it means the DOL has not researched or recorded the vehicle's range. All zeros were replaced with `NaN` in the primary `Electric Range` column, while the original values are preserved in `Electric Range Raw`. All KPI and statistical calculations use the NaN-excluded subset (101,275 rows).

### 3.2 Row Reduction Logic
| Stage | Row Count | Action |
|---|---|---|
| Raw dataset loaded | 280,813 | — |
| After geographic NaN fills + Postal Code fix | 280,813 | No rows dropped |
| After `Electric Range Raw` copy + NaN substitution | 280,813 | No rows dropped |
| After dropping rows with `Electric Range Raw = NaN` | **101,275** | 179,538 rows removed |
| After dropping rows with missing Lat/Long | **101,275** | Minor additional drops (~few hundred) |
| **Final analytical dataset** | **101,275** | Ready for Tableau |

### 3.3 Geographic Precision
Latitude and Longitude were extracted using a regex helper function (`extract_coords`) from the `Vehicle Location` POINT string. Records where the POINT string was corrupt, malformed, or null could not be parsed and were dropped to maintain map integrity. A small number of coordinate outliers exist (lat < 45 or lon > −116) — these are likely data entry errors in the source registry.

### 3.4 Multi-Jurisdiction Utility Zones
Many registration areas are served by overlapping utility companies, indicated by `\|\|` separators in the `Electric Utility` field (e.g., `PUGET SOUND ENERGY INC||CITY OF TACOMA - (WA)`). This multi-agency structure is the basis for the `Utility_Complexity_Score` feature and is a key focus of the infrastructure friction analysis in Dashboard 2.

### 3.5 PHEV Range Under-Representation
PHEVs are disproportionately represented in the rows dropped (range not researched). The final 101,275-row dataset therefore skews toward BEVs in range-based analyses. This is documented in the statistical analysis section and must be noted when interpreting BEV vs PHEV range comparisons.

### 3.6 Market Segment Classification
`Market_Segment` is assigned at the brand level, not by MSRP. This means entry-level luxury models (e.g., BMW 2-series PHEV) and top-spec mass-market models (e.g., Chevrolet Bolt EUV Premium) are classified by brand alone. Future versions should incorporate MSRP data for more precise segmentation.

---

## Part 4 — Tableau Field Binding Reference

When reconnecting the data source in Tableau Desktop, ensure fields are assigned exactly as follows:

| Tableau Field Name | CSV Column Name | Data Type | Geographic Role |
|---|---|---|---|
| VIN | VIN (1-10) | String | — |
| County | County | String | — |
| City | City | String | — |
| State | State | String | — |
| Postal Code | Postal Code | String | — |
| Model Year | Model Year | Integer | — |
| Make | Make | String | — |
| Model | Model | String | — |
| EV Type | Electric Vehicle Type | String | — |
| CAFV Eligibility | Clean Alternative Fuel Vehicle (CAFV) Eligibility | String | — |
| Electric Range | Electric Range | Float | — |
| Legislative District | Legislative District | Float | — |
| DOL Vehicle ID | DOL Vehicle ID | Integer | — |
| Vehicle Location | Vehicle Location | String | — |
| Electric Utility | Electric Utility | String | — |
| Census Tract | 2020 Census Tract | Float | — |
| Electric Range Raw | Electric Range Raw | Float | — |
| Vehicle Age | Vehicle Age | Integer | — |
| CAFV Status | CAFV_Status | String | — |
| Range Category | Range_Category | String | — |
| Latitude | Latitude | Float | **Latitude** |
| Longitude | Longitude | Float | **Longitude** |
| Market Segment | Market_Segment | String | — |
| Eligibility Status | Eligibility_Status | String | — |
| Adoption Wave | Adoption_Wave | String | — |
| Utility Complexity Score | Utility_Complexity_Score | Integer | — |

---

## Data Source Reference
Original dataset available at: https://catalog.data.gov/dataset/electric-vehicle-population-data

*Washington State Department of Licensing · Updated April 2026 · Newton School of Technology — DVA Capstone 2 · Section C Group 6*