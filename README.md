# EV Adoption Analytics: Washington State Electric Vehicle Population Intelligence

> **Newton School of Technology | Data Visualization & Analytics | Capstone 2**
> A comprehensive analytics project using Python, GitHub, and Tableau to convert raw EV registration data into actionable infrastructure intelligence.

---

## Project Overview

| Field | Details |
|---|---|
| **Project Title** | EV Adoption Analytics: Washington State Electric Vehicle Population Intelligence |
| **Sector** | Transportation & Energy Policy |
| **Team ID** | Section-C_G-6 |
| **Section** | Section C |
| **Institute** | Newton School of Technology |
| **Submission Date** | April 2026 |

### Team Members

| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Omved Nagre | `OmvedNagre` |
| Data Lead | Omved Nagre | `OmvedNagre` |
| ETL Lead | Gogulamudi JayaDeep | `Jayadeep007` |
| Analysis Lead | Sushant | `Sushantydv01` |
| Visualization Lead | Om Mishra, Harsh Raj | `Lalilal0`, `dev-harsh-118` |
| Strategy Lead | Harsh Raj, Om Mishra | `dev-harsh-118`, `Lalilal0` |
| PPT and Quality Lead | Anshuman Mehta | `Anshuman-utd` |

---

## Business Problem

Washington State is the leading EV market in the USA, yet policymakers, utility providers, and automakers lack a unified analytical view of where adoption is happening, which vehicle technologies are winning, and which infrastructure bottlenecks need immediate attention. Without granular, spatially-aware analytics, infrastructure investment risks being misallocated—either over-serving already-saturated urban counties or failing to prepare rural areas for imminent demand surges.

**Core Business Question**
> *Which counties, vehicle segments, and technology tiers should Washington State prioritise for infrastructure investment and incentive design to maximise equitable EV adoption by 2030?*

**Decision Supported**
> *Infrastructure investment prioritisation by the WA State Office of Clean Energy and grid upgrade planning by regional Electric Utility Providers.*

---

## Dataset

| Attribute | Details |
|---|---|
| **Source Name** | Washington State Department of Licensing |
| **Direct Access Link** | [Data.gov EV Population Data](https://catalog.data.gov/dataset/electric-vehicle-population-data) |
| **Row Count** | 280,833 (Raw) / 101,275 (Cleaned/Verified Range) |
| **Column Count** | 26 (17 Original + 9 Engineered) |
| **Time Period Covered** | Model Year 1999 to 2026 |
| **Format** | CSV |

**Key Columns Used**

| Column Name | Description | Role in Analysis |
|---|---|---|
| `County` | WA county of registration | Used for geographic segmentation and infrastructure planning |
| `Make` & `Model` | Vehicle manufacturer and model | Used for brand competitiveness and market share analysis |
| `Electric Range` | EPA-rated electric-only range | Primary metric for battery capability progression |
| `Utility_Complexity_Score`| Derived: Count of multi-agency utilities | Used as a proxy for administrative friction in grid upgrades |
| `CAFV_Status` | Clean Alternative Fuel Vehicle eligibility | Used to assess incentive program effectiveness |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| **BEV Adoption %** | Share of Battery Electric Vehicles | `COUNT(BEV) / COUNT(All)` |
| **Avg Electric Range** | Mean EPA range across all vehicles | `AVG(Electric Range)` (Excluding 'not researched' zeros) |
| **CAFV Eligibility %** | Share of CAFV-eligible vehicles | `COUNT(Eligible) / COUNT(All)` |
| **Luxury Segment %** | Share of luxury-brand EVs | `COUNT(Luxury) / COUNT(All)` |
| **Avg Utility Complexity** | Mean utility jurisdiction score | `AVG(Utility_Complexity_Score)` |

Document KPI logic clearly in `notebooks/04_statistical_analysis.ipynb` and `notebooks/05_final_load_prep_tableau.ipynb`.

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | `EV_Adoption_Analytics.twbx` (To be published to Tableau Public) |
| **Executive View** | **Dashboard 1:** High-level KPI summary, technology split, adoption wave trends, and brand dominance. |
| **Operational View** | **Dashboard 2:** Geographic map view showing spatial concentration and utility complexity. |
| **Deep Dive View** | **Dashboard 3:** Analysis of range capability progression, CAFV eligibility, and model-level tracking. |
| **Main Filters** | `EV Type`, `Make`, `County`, `Market Segment`, `Model Year`, `Range Category` |

Store dashboard screenshots in [`tableau/screenshots/`](tableau/screenshots/) and document the public links in [`tableau/dashboard_links.md`](tableau/dashboard_links.md).

---

## Key Insights

1. **BEV technology has decisively overtaken PHEV in verified-range performance** — BEVs account for 68.7% of verified-range vehicles, signalling a shift away from hybrid solutions.
2. **Tesla dominates at 23.9%** of all registrations — no other brand exceeds 10%, indicating a highly monopolised pure-BEV market.
3. **King County holds 46% of all EVs** — massive geographic concentration in the Seattle metro means rest-of-state requires aggressive stimulation.
4. **82.1% of verified-range vehicles are CAFV-eligible**, validating the current clean fuel policy incentive structure.
5. **Luxury segment accounts for nearly half (48.1%)** of verified vehicles, signalling that EV adoption in WA State remains significantly skewed toward high-income buyers.
6. **Battery range has improved 6× since 2010** — from ~30 mi to ~200 mi average for long-range BEVs, making range anxiety an increasingly obsolete barrier.
7. **The Mass Adoption wave (2021+) is accelerating** — 37,530 vehicles in just 4–5 model years vs 50,005 across the full 5-year Growth wave.
8. **Infrastructure friction is highest in rural counties** — Asotin, Mason, and Jefferson have the highest utility complexity scores (3.0) despite low EV density, representing a hidden bottleneck.

---

## Recommendations

| # | Insight Addressed | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | Geographic concentration; Snohomish/Pierce growth | Deploy Level 2 and DC fast charging infrastructure in Snohomish, Pierce, and Clark counties with a density target of 1 charger per 50 registered EVs. | 15–20% increase in EV registrations in these target counties within 24 months. |
| 2 | Luxury-skewed adoption; mass-market gap | Create a $3,000 state point-of-sale rebate for mass-market BEVs priced under $40,000 (targeting LEAF, BOLT EV, Kia EV6 segment). | Model projects 12,000+ additional mass-market BEV registrations in Year 1. |
| 3 | 17.9% CAFV ineligibility; PHEV short-range gap | Extend CAFV incentive eligibility to PHEVs with ≥ 20 mi range through a separate "Transition Fuel" tier. | Increases eligible fleet to 95%+, improving consumer confidence in purchasing PHEVs as a stepping stone. |
| 4 | Rural high-friction utility complexity | Mandate pre-emptive multi-agency grid coordination MOUs in counties with Utility Complexity Score ≥ 2.5 before EV density exceeds 500 vehicles/county. | Reduces future infrastructure coordination cost by an estimated 30–40% compared to reactive upgrades. |

---

## Project Structure & Documentation

- [Detailed Project Report](reports/project_report_template.md)
- [Data Dictionary](docs/data_dictionary.md)
- [Tableau Field Binding Specification](docs/tableau_dataset_specification.md)
