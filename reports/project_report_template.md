## EV Adoption Analytics: Washington State Electric Vehicle Population Intelligence

> Section C | Group 6 | Submission: April 2026

---

## 1. Cover Page

| Field | Details |
|---|---|
| **Project Title** | EV Adoption Analytics: Washington State Electric Vehicle Population Intelligence |
| **Sector** | Transportation & Energy Policy |
| **Team ID** | Section-C\_G-6 |
| **Institute** | Newton School of Technology |
| **Dataset Source** | Washington State Dept. of Licensing via [Data.gov](https://catalog.data.gov/dataset/electric-vehicle-population-data) |
| **Submission Date** | April 2026 |
| **Tableau Workbook** | `EV_Adoption_Analytics.twbx` |
| **Processed Dataset** | `data/processed/EV_Final_Ready_Tableau.csv` |

### Team Members
| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Omved Nagre | OmvedNagre |
| Data Lead | Omved Nagre | OmvedNagre |
| ETL Lead | Gogulamudi JayaDeep | Jayadeep007 |
| Analysis Lead | Sushant | Sushantydv01 |
| Visualization Lead | Om Mishra, Harsh Raj | Lalilal0, dev-harsh-118 |
| Strategy Lead | Harsh Raj, Om Mishra | dev-harsh-118, Lalilal0 |
| PPT and Quality Lead | Anshuman Mehta | Anshuman-utd |

---

## 2. Executive Summary

### Problem
Washington State is the leading EV market in the USA, yet policymakers, utility providers, and automakers lack a unified analytical view of where adoption is happening, which vehicle technologies are winning, and which infrastructure bottlenecks need immediate attention.

### Approach
We sourced the complete Washington State EV registration registry (280,833 raw records) from the Department of Licensing, performed a rigorous 5-stage ETL pipeline, engineered 9 derived analytical features, and built three interactive Tableau dashboards covering executive KPIs, geospatial intelligence, and market & technology analysis.

### Key Insights 
1. **BEV technology is winning** — BEVs account for 68.7% of verified-range vehicles, up sharply from near-zero pre-2013.
2. **Tesla dominates at 23.9%** of all registrations — more than double its nearest competitor.
3. **King County holds 46% of all EVs** — massive geographic concentration in the Seattle metro.
4. **82.1% of verified-range vehicles are CAFV-eligible**, validating the policy incentive structure.
5. **Luxury segment at 48.1%** signals that EVs remain predominantly an upper-income adoption story.
6. **Infrastructure friction is highest in rural counties** (Asotin, Mason, Jefferson avg score 3.0) despite low EV density.
7. **Battery range has improved 6× since 2010** — from ~30 mi to ~200 mi average for long-range BEVs.
8. **Mass Adoption wave (2021+) is accelerating** — 37,530 vehicles in just 4–5 model years vs 50,005 across the full 5-year Growth wave.

### Key Recommendations
1. Prioritise charging infrastructure investment in Snohomish, Pierce, and Clark counties — highest growth outside King.
2. Extend CAFV incentives to mass-market PHEVs to convert the 17.9% ineligible segment.
3. Target grid upgrade partnerships in high-friction counties (Asotin, Mason, Jefferson) before EV demand arrives.
4. Design affordable BEV incentive programs to shift the 51.9% mass-market share toward full electrification.
5. Use the Tesla Model 3 and LEAF as benchmark adoption signals for regional demand forecasting.

---

## 3. Sector & Business Context

### Sector Overview
The global EV transition is the largest structural shift in automotive history. Washington State leads US adoption driven by progressive clean-energy legislation (Clean Cars 2030 Act), state EV tax credits, and a high-income, tech-forward population concentrated in the Puget Sound corridor. The state mandates all new passenger car sales be zero-emission by 2035.

### Decision-Makers / Stakeholders
| Stakeholder | Decision Supported |
|---|---|
| WA State Office of Clean Energy | Infrastructure investment prioritisation |
| Electric Utility Providers | Grid upgrade planning by county |
| Auto Manufacturers | Market entry strategy, model mix decisions |
| City & County Planners | Charging station siting and zoning |
| Policy Analysts | Incentive program design and evaluation |

### Why This Problem Matters
Without granular, spatially-aware analytics, infrastructure investment risks being misallocated — either over-serving already-saturated urban counties or failing to prepare rural areas for imminent demand surges. Every year of delayed grid investment in high-friction counties adds disproportionate coordination cost when EV density eventually arrives.

### Core Business Question
> *Which counties, vehicle segments, and technology tiers should Washington State prioritise for infrastructure investment and incentive design to maximise equitable EV adoption by 2030?*

---

## 4. Problem Statement & Objectives

### Formal Problem Definition
Washington State possesses detailed EV registration data but lacks a decision-ready analytical framework that simultaneously answers: **Where** are EVs concentrated? **Which** technologies and brands are leading? **Who** is adopting — luxury or mass-market buyers? **Where** is grid infrastructure under-prepared?

### Scope
- Geography: Washington State (all 39 counties, 500+ cities)
- Time period: Model Year 1999 – 2026
- Dataset: 280,833 raw registration records → 101,275 verified-range analytical records
- Technology: Python ETL + Tableau Public dashboards

### Success Criteria
- Deliver 3 interactive Tableau dashboards answering geographic, executive, and market dimensions
- Produce 8–12 data-backed insights in decision language
- Generate 3–5 actionable recommendations with measurable expected impact
- Complete full ETL pipeline with documented data dictionary and cleaning log

---

## 5. Data Description

### Source
| Attribute | Detail |
|---|---|
| **Name** | Electric Vehicle Population Data |
| **Provider** | Washington State Department of Licensing |
| **Access** | https://catalog.data.gov/dataset/electric-vehicle-population-data |
| **Format** | CSV, UTF-8, comma-delimited |
| **Raw Row Count** | 280,833 |
| **Analytical Row Count** | 101,275 (verified electric range only) |
| **Columns** | 26 (17 original + 9 engineered) |
| **Granularity** | One row = one registered electric vehicle |
| **Time Span** | Model Year 1999 – 2026 |

### Key Original Columns

| Column | Type | Description |
|---|---|---|
| `County` | String | WA county of registration (170 found, 39 standard) |
| `City` | String | City of registration |
| `Model Year` | Integer | Manufacture year (1999–2026) |
| `Make` | String | Vehicle manufacturer (36 unique brands) |
| `Model` | String | Vehicle model name |
| `Electric Vehicle Type` | String | BEV or PHEV |
| `Electric Range` | Float | EPA electric-only range in miles |
| `CAFV Eligibility` | String | Clean Alternative Fuel Vehicle program status |
| `Electric Utility` | String | Utility provider(s), `\|\|`-separated for multi-jurisdiction |
| `Vehicle Location` | String | `POINT(lon lat)` coordinate string |
| `Legislative District` | Float | WA State legislative district (1–49) |

### Data Quality Issues Found
| Issue | Scale | Resolution |
|---|---|---|
| Electric Range = 0 ("not researched") | ~64% of raw rows (179,558) | Replaced with NaN; retained in `Electric Range Raw` for population counts |
| Postal Code stored as float (98034.0) | All rows | Converted to integer string |
| Missing County / City / Legislative District | Minor (<1%) | Filled with `"Unknown"` to preserve grouping integrity |
| Corrupt POINT coordinate strings | ~500 rows | Dropped — required for map integrity |
| CAFV eligibility in verbose raw text | All rows | Simplified to 3-tier `Eligibility_Status` |

---

## 6. Cleaning & Transformation

### Step 1 — Initial Setup & Data Loading

**Libraries used:** `pandas`, `numpy`

The raw dataset `Electric_Vehicle_Population_Data.csv` was loaded into a pandas DataFrame. A `DtypeWarning` for Column 11 (`Electric Range`) was noted due to mixed types — handled implicitly by pandas during read.

| Item | Detail |
|---|---|
| Raw file loaded | `Electric_Vehicle_Population_Data.csv` |
| Initial shape | (280,813 rows × 17 columns) |
| Warning noted | DtypeWarning on Column 11 — mixed types in Electric Range |

---

### Step 2 — Missing Geographic Data

Missing values in the following geographic columns were filled with the string `'Unknown'` to prevent row loss during grouping and aggregation:

| Column Filled | Reason |
|---|---|
| `County` | Required for county-level grouping and choropleth map |
| `City` | Required for city-level drill-down filter |
| `Postal Code` | Required for ZIP-level mapping |
| `Legislative District` | Required for policy-level analysis |
| `Vehicle Location` | Required for coordinate extraction |

---

### Step 3 — Postal Code Standardisation

The `Postal Code` column was originally stored as a float (e.g., `98034.0`) due to how pandas inferred types from CSV. It was standardised by:
1. Converting to string type
2. Stripping the trailing `.0` suffix (e.g., `98034.0` → `98034`)

This ensures correct ZIP-level mapping in Tableau without decimal artifacts in labels.

---

### Step 4 — Handling Electric Range

**`Electric Range Raw` column created** — an exact copy of the original `Electric Range` column was preserved as `Electric Range Raw` before any transformation. This retains the original values (including zeros) for population count analyses.

**Zero value treatment** — All zero values in the primary `Electric Range` column were replaced with `np.nan`. This is because a `0` value in the DOL registry indicates **"range not researched"**, not a literal zero-mile vehicle. Using these zeros in statistical analyses would artificially deflate the average range and skew distributions.

| Before Treatment | After Treatment |
|---|---|
| 179,538 rows with `Electric Range = 0` | Replaced with `NaN` |
| Mean range (incl. zeros) | Meaningless / deflated |
| Mean range (NaN-excluded) | **107.5 mi** — accurate fleet average |

---

### Step 5 — Feature Engineering (KPI Preparation)

#### 5a. Vehicle Age

- Range: 0 (2026 model) to 27 (1999 model)
- Purpose: Fleet aging analysis, age-vs-range correlation

#### 5b. CAFV_Status
The verbose `Clean Alternative Fuel Vehicle (CAFV) Eligibility` column was simplified:

| Value | Count |
|---|---|
| `Eligible` | 76,897 |
| `Unknown/Not Eligible` | 24,074 |

---

### Step 6 — Advanced Feature Engineering for Tableau

#### 6a. Range_Category
A categorical column derived from `Electric Range Raw` using predefined bins:

| Bin | Label | Count |
|---|---|---|
| `[-1, 0]` | `Unknown/Not Researched` | 179,538 (raw data only) |
| `(0, 100]` | `Urban Commuter` | 63,848 |
| `(100, 250]` | `Regional Traveler` | 27,529 |
| `(250, 1000]` | `Long Range` | 9,594 |

#### 6b. Geospatial — Latitude & Longitude
A helper function `extract_coords` was defined using **regular expressions** to parse the `Vehicle Location` POINT string:

```python
# Example input:  POINT (-122.22901 47.72201)
# Outputs: Longitude = -122.22901, Latitude = 47.72201
```

Two new numeric columns `Latitude` and `Longitude` were created, enabling geographical scatter maps and heatmaps in Tableau.

#### 6c. Market_Segment — Brand Segmentation

| Segment | Count | Share |
|---|---|---|
| `Mass Market` | 64,595 | 64% |
| `Luxury` | 36,376 | 36% |

#### 6d. Eligibility_Status — Traffic Light Classification
The CAFV eligibility field was mapped into a clean 3-tier status:

| Input Text Contains | Assigned Status |
|---|---|
| `"Eligible"` | `Eligible` |
| `"not eligible"` / low range | `Not Eligible` |
| `"unknown"` / not researched | `Unknown` |

Final distribution: **Eligible (76,897)** · **Not Eligible (24,074)**


#### 6e. Utility_Complexity_Score — Infrastructure Friction

- Score of `1` = single utility (e.g., `PUGET SOUND ENERGY INC`)
- Score of `2` = two utilities (e.g., `PUGET SOUND ENERGY INC||CITY OF TACOMA`)
- Score of `3` = three or more utility jurisdictions
- Higher score = more administrative coordination required for grid upgrades

---

### Step 7 — Dropping Rows with Missing Electric Range

**Decision rationale:** 179,538 out of 280,813 rows had `NaN` in `Electric Range Raw` — meaning the DOL had not researched the range for these vehicles. For range-specific KPIs and analyses, including these records would produce meaningless averages.

**Action:** All rows where `Electric Range Raw` is `NaN` were dropped.

| Stage | Shape |
|---|---|
| Before drop | (280,813 × 26) |
| After drop | **(101,275 × 26)** |
| Rows retained | 101,275 (~36.06% of original) |

This analytical subset is the dataset used for all KPIs, statistical tests, and Tableau dashboards.

---

### Step 8 — Final Cleanup & Export

**Geospatial filtering:** Rows where `Latitude` or `Longitude` could not be extracted (corrupt or null POINT strings) were removed to ensure map integrity.

**Final export:**

| Property | Value |
|---|---|
| Output filename | `EV_Final_Ready_Tableau.csv` |
| Final shape | 101,275 rows × 26 columns |
| Encoding | UTF-8 |
| Also exported | `EV_Population_Cleaned.csv` (intermediate cleaned file) |

---

### Summary: All 9 Engineered Columns

| # | Column | Source | Logic | Purpose |
|---|---|---|---|---|
| 1 | `Electric Range Raw` | `Electric Range` | Copy before NaN treatment | Preserve original values for population counts |
| 2 | `Vehicle Age` | `Model Year` | `2026 − Model Year` | Fleet aging analysis |
| 3 | `CAFV_Status` | `CAFV Eligibility` | Contains 'Eligible' → 'Eligible' else 'Unknown/Not Eligible' | Simplified incentive KPI |
| 4 | `Range_Category` | `Electric Range Raw` | Bins: 0–100 / 101–250 / 251+ | Human-readable range tier |
| 5 | `Latitude` | `Vehicle Location` | Regex extraction from POINT string | Geospatial mapping |
| 6 | `Longitude` | `Vehicle Location` | Regex extraction from POINT string | Geospatial mapping |
| 7 | `Market_Segment` | `Make` | Luxury brand list → 'Luxury'; else 'Mass Market' | Socio-economic accessibility |
| 8 | `Eligibility_Status` | `CAFV Eligibility` | 3-tier mapping: Eligible / Not Eligible / Unknown | Dashboard traffic-light filter |
| 9 | `Adoption_Wave` | `Model Year` | ≤2015 Early; 2016–2020 Growth; 2021+ Mass Adoption | Transition velocity analysis |
| 10 | `Utility_Complexity_Score` | `Electric Utility` | Count of `\|`-split segments | Infrastructure friction proxy |

---

## 7. KPI Framework

| KPI | Definition | Formula | Value (Verified Range Dataset) |
|---|---|---|---|
| **Total Registrations** | All verified-range EV records | `COUNT(DOL Vehicle ID)` | **101,275** |
| **BEV Adoption %** | Share of Battery Electric Vehicles | `COUNT(BEV) / COUNT(All)` | **68.7%** (Dashboard 1 filtered view) |
| **Avg Electric Range** | Mean EPA range across all vehicles | `AVG(Electric Range)` | **151.7 mi** |
| **CAFV Eligibility %** | Share of CAFV-eligible vehicles | `COUNT(Eligible) / COUNT(All)` | **82.1%** |
| **Luxury Segment %** | Share of luxury-brand EVs | `COUNT(Luxury) / COUNT(All)` | **48.1%** (filtered view) |
| **Top County Share** | King County as % of total | `COUNT(King) / COUNT(All)` | **46.0%** |
| **Tesla Market Share** | Tesla as % of all makes | `COUNT(Tesla) / COUNT(All)` | **23.9%** |
| **Mass Adoption Count** | Vehicles from 2021+ Model Year | `COUNT(Adoption_Wave = 'Mass Adoption')` | **37,530** |
| **Avg Utility Complexity** | Mean utility jurisdiction score | `AVG(Utility_Complexity_Score)` | **1.8** |

---

## 8. Exploratory Analysis

### Major Trends

**Adoption Curve (1999–2026)**
EV registrations in Washington State followed a classic S-curve. Growth was negligible pre-2010 (< 200 vehicles/year). The 2013–2016 period marked inflection — Nissan LEAF democratised BEV ownership. The 2018–2019 Tesla Model 3 launch triggered exponential acceleration. The 2020–2021 dip reflects supply chain disruptions; 2022–2026 shows resumed rapid growth with mass-market PHEVs entering at scale.

| Adoption Wave | Model Years | Count | Share |
|---|---|---|---|
| Early Adopters | ≤ 2015 | 13,436 | 13% |
| Growth Phase | 2016 – 2020 | 50,005 | 49% |
| Mass Adoption | 2021+ | 37,530 | 37% |

**Geographic Concentration**
The top 5 counties account for 76% of all registrations — a highly concentrated urban adoption pattern.

| County | Registrations | Share |
|---|---|---|
| King | 46,605 | 46.0% |
| Snohomish | 10,718 | 10.6% |
| Pierce | 8,590 | 8.5% |
| Clark | 6,814 | 6.7% |
| Thurston | 4,103 | 4.1% |

**Top Cities:** Seattle (16,230) → Vancouver (4,254) → Bellevue (3,827) → Olympia (2,668) → Redmond (2,645)

**Manufacturer Landscape**

| Make | Count | EV Type Focus |
|---|---|---|
| TESLA | 24,182 | BEV only |
| CHEVROLET | 9,848 | Mixed (BOLT EV + VOLT PHEV) |
| TOYOTA | 9,750 | PHEV dominant (Prius Prime, RAV4 Prime) |
| NISSAN | 9,631 | BEV dominant (LEAF) |
| BMW | 7,048 | Mixed (i3 BEV + PHEV models) |
| JEEP | 6,568 | PHEV dominant (Wrangler 4xe) |

**Vehicle Type Split**
- BEV: 45,583 (45% of full dataset; 68.7% in verified-range filtered view)
- PHEV: 55,692 (55% of full dataset; 31.3% in verified-range filtered view)

The discrepancy arises because PHEVs disproportionately lack researched range data in the DOL registry.

**Market Segment**
- Mass Market: 64,595 (64%)
- Luxury: 36,376 (36%)

**Range Categories**
| Category | Miles | Count | Share |
|---|---|---|---|
| Urban Commuter | ≤ 50 mi | 63,848 | 63% |
| Regional Traveler | 51–200 mi | 27,529 | 27% |
| Long Range | > 200 mi | 9,594 | 9% |

### Segment-Level Insights
- Luxury BEVs average **256 mi** range vs Mass Market BEVs at **98 mi**
- Tesla Model 3 alone = **~14,000** registrations — single largest model
- Chevrolet BOLT EV is the top mass-market BEV at **~6,000** units
- Nissan LEAF remains the most affordable long-term adoption vehicle
- King County's avg utility complexity = **2.0** (multi-agency grid already in effect)

---

## 9. Statistical Analysis

### Methods Applied

**1. Mann-Whitney U Test — BEV vs PHEV Range**
- H₀: No difference in electric range distribution between BEV and PHEV
- Result: U = 4.2 × 10⁸, **p < 0.001** → Reject H₀
- Interpretation: BEVs have statistically significantly higher electric range. PHEVs cluster at 20–50 mi (Urban Commuter tier); BEVs cluster at 100–330 mi.

**2. One-Way ANOVA — Range across Adoption Waves**
- H₀: Mean electric range is equal across Early, Growth, and Mass Adoption waves
- Result: F(2, 101272) = **847.3**, **p < 0.001** → Reject H₀
- Post-hoc (Tukey): All 3 wave pairs are significantly different
- Interpretation: Each generation of EVs delivers meaningfully higher range — technology progress is statistically confirmed.

**3. Pearson Correlation — Vehicle Age vs Electric Range**
- r = **−0.61**, p < 0.001
- Interpretation: Moderate-strong negative correlation. Older vehicles have substantially shorter range — each additional year of age corresponds to ~8–10 mi lower range on average.

**4. Kruskal-Wallis Test — Range by Market Segment**
- H₀: Range distribution identical for Luxury and Mass Market
- Result: **p < 0.001** → Reject H₀
- Interpretation: Luxury vehicles have a statistically higher range distribution, confirming that premium pricing partially reflects superior battery technology.

**5. Infrastructure Complexity Distribution**
- Mean Utility Complexity Score: **1.8**
- Counties with Score 3.0: Asotin, Mason, Jefferson — all rural, low-EV-density areas
- King County (highest EV density): Score **2.0** — manageable multi-agency coordination already established

---

## 10. Dashboard Walkthrough

### Dashboard 1 — EV Adoption Pulse (Executive Overview)

**Objective**: Provide policymakers and executives a single-screen command view of WA State EV adoption.

**KPI Banner (5 cards across top)**
| KPI Card | Displayed Value | Color |
|---|---|---|
| Total Vehicles | 40,661 (filtered view) | Electric Blue |
| BEV Adoption % | 68.7% | Electric Blue |
| Avg Electric Range | 151.7 mi | Neon Green |
| CAFV Eligibility % | 82.1% | Purple |
| Luxury Segment % | 48.1% | Amber |

**Charts**
- **EV Registrations by Year** (stacked area): Electric Blue (BEV) + Amber (PHEV) — shows the dramatic growth curve from 2013 onwards with Market Segment (Luxury = purple, Mass Market = green) overlaid
- **Top 10 EV Manufacturers** (horizontal bar): Tesla leads at 16,830 in filtered view; sequential blue gradient
- **Vehicle Type Split** (donut): 68.7% BEV (blue) / 31.3% PHEV (amber)
- **Adoption Waves × Market Segment** (stacked bar): Early (5,910) → Growth (16,909 Luxury + 13,140 Mass) → Mass Adoption

**Filters**: Make dropdown, Electric Vehicle Type toggle, Market Segment, Year range slider

**Aesthetic**: Dark navy background (#0A0E1A), electric blue accents, white titles, neon green highlights

---

### Dashboard 2 — Geographic Intelligence (Regional Map)

**Objective**: Identify spatial concentration, regional infrastructure needs, and grid complexity patterns.

**Charts**
- **EV Count by County** (bubble/scatter map on dark basemap): Bubble size = registration count; King County bubble dominates Seattle metro; sparse coverage east of Cascades
- **City-Level EV Distribution** (dot density map): High-precision lat/long scatter; Seattle/Bellevue/Redmond cluster visually prominent; Spokane as the only significant eastern WA cluster
- **Top 10 Counties — BEV vs PHEV** (stacked bar): King (27,486 shown in filtered view) leads massively; BEV (blue) vs PHEV (amber) split visible per county
- **Infrastructure Friction — Avg Utility Complexity** (horizontal bar): Asotin, Mason, Jefferson score 3.0 (highest friction); Clark, King, Cowlitz at 2.0

**Key Geographic Insight Visible**: The utility complexity chart reveals that rural counties with the fewest EVs today have the most complex utility jurisdictions — making future grid upgrades administratively harder precisely where demand will grow next.

**Filters**: County dropdown, Electric Vehicle Type, Limit (Top 15 by complexity), interactive map click-to-filter

---

### Dashboard 3 — Market & Technology Deep Dive

**Objective**: Analyse competitive brand dynamics, battery technology progress, and incentive program effectiveness.

**Charts**
- **Brand Competitiveness — BEV vs PHEV** (grouped horizontal bar): Tesla, Nissan, Chevrolet lead BEV; Toyota, BMW, Jeep, Volvo, Chrysler lead PHEV — reveals clear brand technology positioning
- **Battery Technology Evolution — Avg Range by Year** (dual line): Purple = Luxury, Green = Mass Market; both trend upward; "Sweet Spot" reference band at 100–200 mi; Luxury consistently above
- **CAFV Eligibility Status** (pie): 82.06% Eligible (green), 17.94% Not Eligible (red)
- **Range Capability Evolution by Year** (stacked area): Urban Commuter (gray), Regional Traveler (blue), Long Range (green) — Long Range share visibly expanding post-2017
- **Model Mix — Within Top Brands** (treemap): Tesla Model 3 = largest tile; LEAF, BOLT EV, Prius Prime, Wrangler, Pacifica visible as significant models
- **Top Models Table**: BMW X5 (595 count, 20.9 mi avg range) through Nissan LEAF (6,253 count, 107.5 mi avg) — performance vs volume trade-off visible

**Filters**: Market Segment toggle (Luxury/Mass Market), Year of Model Year slider (2000–2021), Make dropdown, Range Category

---

## 11. Key Insights

1. **BEV technology has decisively overtaken PHEV in verified-range performance** — BEVs average 189 mi vs PHEVs at 32 mi. PHEVs serve a transition role but are not the end-state technology.

2. **Tesla's 23.9% market dominance is structurally unchallenged** — no other brand exceeds 10%. Tesla's Model 3 alone (≈14K units) exceeds all Chevrolet EV models combined.

3. **King County (Seattle metro) absorbs 46% of all EVs** — any infrastructure investment outside this cluster is currently under-utilised relative to need. Snohomish and Pierce represent the highest-ROI next investment zones.

4. **82.1% CAFV eligibility rate validates the current incentive design** for BEVs, but the 17.9% ineligible segment (predominantly short-range PHEVs) represents a policy gap — these buyers purchased EVs but don't qualify for the program.

5. **Luxury brands account for 36–48% of registrations** (depending on filter), revealing that EV adoption in WA State remains significantly skewed toward high-income buyers. Mass-market electrification is the unfinished agenda.

6. **Battery range has improved 6× since 2010** — from a fleet average of ~30 mi to ~180 mi. This validates continued investment in BEV technology and signals that range anxiety is an increasingly obsolete barrier.

7. **The Mass Adoption wave (2021+) is outpacing the Growth wave** on a per-year basis — 37,530 vehicles across 4–5 model years vs 50,005 across 5 model years. The tipping point has been crossed.

8. **Rural counties (Asotin, Mason, Jefferson) have the highest utility complexity scores (3.0)** despite having the fewest EVs. These areas require proactive multi-agency grid coordination agreements before EV demand arrives.

9. **Nissan LEAF is the most accessible mass-market BEV** — 9,631 registrations at a consistent price point; a bellwether for price-sensitive EV adoption and a signal that incentive design should target the $25K–$35K segment.

10. **Vehicle age strongly predicts electric range** (r = −0.61) — the oldest vehicles are the shortest-range. As fleet turnover accelerates, Washington's average range will improve naturally without policy intervention.

11. **PHEVs dominate rural registrations** — Yakima, Spokane, and eastern WA counties show higher PHEV proportions, likely reflecting longer intercity drives where full-BEV range remains a concern. Rural charging infrastructure is the missing enabler.

12. **The "Growth wave" (2016–2020) produced 49% of all registrations** and is now aging into prime replacement years (Vehicle Age 6–10). A replacement demand surge is forecast for 2026–2030 — automakers and utilities should prepare for a fleet renewal cycle.

---

## 12. Recommendations

| # | Insight Addressed | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | Geographic concentration; Snohomish/Pierce growth | Deploy Level 2 and DC fast charging infrastructure in Snohomish, Pierce, and Clark counties with a density target of 1 charger per 50 registered EVs | 15–20% increase in EV registrations in these counties within 24 months |
| 2 | 17.9% CAFV ineligibility; PHEV short-range gap | Extend CAFV incentive eligibility to PHEVs with ≥ 20 mi range through a separate "Transition Fuel" tier; this captures ~18,000 current registrations | Increases eligible fleet to 95%+, improving consumer confidence in purchasing PHEVs as a stepping stone |
| 3 | Rural high-friction utility complexity | Mandate pre-emptive multi-agency grid coordination MOUs in counties with Utility Complexity Score ≥ 2.5 before EV density exceeds 500 vehicles/county | Reduces future infrastructure coordination cost by est. 30–40% compared to reactive upgrades |
| 4 | Luxury-skewed adoption; mass-market gap | Create a $3,000 state point-of-sale rebate for mass-market BEVs priced under $40,000 (targeting LEAF, BOLT EV, Kia EV6 segment) | Model projects 12,000+ additional mass-market BEV registrations in Year 1 |
| 5 | Fleet replacement wave approaching (2026–2030) | Engage fleet operators and dealers with trade-in incentive programs for vehicles with Model Year ≤ 2016 (Vehicle Age ≥ 10) to accelerate replacement with modern long-range BEVs | Reduces average fleet age by 2–3 years; lifts average fleet range by ~40 mi |

---

## 13. Limitations & Next Steps

### Data Limitations
- **64% missing range data** in the raw dataset means population-level range analysis is based on a subset. PHEVs are disproportionately missing, skewing BEV/PHEV range comparisons.
- **Administrative registry data** reflects registration location, not where vehicles are actually driven. A King County registered vehicle may regularly commute to Snohomish — charging demand is undercounted outside urban cores.
- **No income or demographic data** is linked to registrations, making direct socio-economic equity analysis impossible without external data join.
- **Snapshot, not panel** — the dataset captures the current registry state; historical deregistrations and vehicle retirements are not tracked.

### Method Limitations
- Non-parametric statistical tests were used due to non-normal range distributions — effect sizes are not directly comparable across groups.
- Market Segment classification is based on brand-level assignment, not MSRP, which may misclassify some entry-level luxury or top-trim mass-market vehicles.

### Suggested Future Work
1. **Join with Census income data** (ACS) by census tract to produce equity-adjusted adoption maps
2. **Time-series forecasting** (ARIMA / Prophet) on monthly registration counts to project 2027–2030 adoption
3. **Charging infrastructure gap analysis** — overlay with publicly available AFDC charger location data
4. **Panel data tracking** — request historical DOL data to analyse deregistration and fleet turnover patterns
5. **Grid capacity modelling** — combine Utility Complexity Score with utility published capacity data for infrastructure stress-testing

---

## 14. Repository Structure

```
Section-C_G-6_EV-Adoption-Analytics/
│
├── README.md                          ← Project overview, team, and KPI summary
│
├── data/
│   ├── raw/
│   │   └── Electric_Vehicle_Population_Data.csv    ← Original DOL dataset (280,833 rows)
│   └── processed/
│       ├── EV_Population_Cleaned.csv               ← Post-cleaning, pre-feature engineering
│       ├── EV_Final_Ready_Tableau.csv              ← Final 26-column analytical dataset (101,275 rows)
│       └── cleaning_insights.md                    ← ETL decisions and data quality log
│
├── notebooks/
│   ├── 01_extraction.ipynb
│   ├── 02_Cleaning.ipynb
│   ├── 03_eda_.ipynb
│   ├── 04_statistical_analysis.ipynb
│   └── 05_final_load_prep_tableau.ipynb
│
├── scripts/
│   └── etl_pipeline.py                ← Standalone ETL script
│
├── tableau/
│   ├── screenshots/                   ← Dashboard PNG exports
│   └── dashboard_links.md             ← Tableau Public URLs
│
├── docs/
│   ├── data_dictionary.md             ← Column definitions and cleaning notes
│   └── tableau_dataset_specification.md  ← Full 26-column field binding spec
│
└── reports/
    ├── README.md                      ← This file (full project report)
    ├── project_report_template.md     ← PDF export drafting template
    └── presentation_outline.md        ← Presentation deck structure
```

---

## 15. Contribution Matrix

| Team Member | Role | Dataset Sourcing | ETL & Cleaning | EDA & Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT & Viva |
|---|---|---|---|---|---|---|---|---|
| Omved Nagre | Project & Data Lead | Owner | Support | Owner | Owner | Support | Owner | Support |
| Gogulamudi JayaDeep | ETL Lead | Support | Owner | Support | Support | Support | Support | Support |
| Sushant | Analysis Lead | Support | Support | Owner | Owner | Support | Support | Support |
| Om Mishra | Viz & Strategy Lead | Support | Support | Support | Support | Owner | Support | Support |
| Harsh Raj | Viz & Strategy Lead | Support | Support | Support | Support | Owner | Support | Support |
| Anshuman Mehta | PPT & Quality Lead | Support | Support | Support | Support | Support | Support | Owner |

---

## 16. Tech Stack

| Tool | Version | Purpose |
|---|---|---|
| Python | 3.11+ | ETL, cleaning, analysis |
| pandas | 2.x | Data wrangling |
| numpy | 1.x | Numeric computation |
| matplotlib / seaborn | Latest | EDA visualisation |
| scipy / statsmodels | Latest | Statistical testing |
| Jupyter Notebooks | 7.x | Interactive analysis environment |
| Tableau Public | 2024.x | Dashboard design and publishing |
| GitHub | — | Version control, collaboration |

---

*Newton School of Technology — Data Visualization & Analytics | Capstone 2 | Section C Group 6*
