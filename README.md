## Project Overview

| Field | Details |
|---|---|
| **Project Title** | EV Adoption Analytics: Washington State Electric Vehicle Population Intelligence |
| **Sector** | Transportation & Energy Policy |
| **Team ID** | G-6 |
| **Section** | Section C |
| **Faculty Mentor** | _To be filled by team_ |
| **Institute** | Newton School of Technology |
| **Submission Date** | April 2026 |

### Team Members

| Role | Name | GitHub Username |
|---|---|---|
| Project Lead | Omved Nagre | `OmvedNagre` |
| Data Lead | Om Mishra | `Lalilal0` |
| ETL Lead | Gogulamudi JayaDeep | `Jayadeep007` |
| Analysis Lead | Sushant | `Sushantydv01` |
| Visualization Lead | Harsh Raj | `dev-harsh-118` |
| Strategy + Quality Lead | Anshuman Mehta | `Anshuman-utd` |

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
| `County` | WA county of registration | Geographic segmentation and infrastructure planning |
| `Make` & `Model` | Vehicle manufacturer and model | Brand competitiveness and market share |
| `Electric Range` | EPA-rated electric-only range | Battery capability progression tracking |
| `Utility_Complexity_Score` | Count of multi-agency utilities | Proxy for administrative friction in grid upgrades |

For full column definitions, see [`docs/data_dictionary.md`](docs/data_dictionary.md).

---

## KPI Framework

| KPI | Definition | Formula / Computation |
|---|---|---|
| **BEV Adoption %** | Share of Battery Electric Vehicles | `COUNT(BEV) / COUNT(All)` |
| **Avg Electric Range** | Mean EPA range across all vehicles | `AVG(Electric Range)` (Excluding zeros) |
| **CAFV Eligibility %** | Share of CAFV-eligible vehicles | `COUNT(Eligible) / COUNT(All)` |
| **Utility Complexity** | Mean utility jurisdiction score | `AVG(Utility_Complexity_Score)` |

Document KPI logic clearly in `notebooks/04_statistical_analysis.ipynb` and `notebooks/05_final_load_prep_tableau.ipynb`.

---

## Tableau Dashboard

| Item | Details |
|---|---|
| **Dashboard URL** | `EV_Adoption_Analytics.twbx` (To be published to Tableau Public) |
| **Executive View** | High-level KPI summary, technology split, adoption wave trends, and brand dominance. |
| **Operational View** | Geographic map view showing spatial concentration and utility complexity per county. |
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

| # | Insight | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | Geographic concentration | Deploy Level 2 and DC fast charging in Snohomish, Pierce, and Clark counties with a density target. | 15–20% increase in EV registrations in these target counties within 24 months. |
| 2 | Luxury-skewed adoption | Create a state point-of-sale rebate for mass-market BEVs priced under $40,000 (targeting LEAF, BOLT EV). | Model projects 12,000+ additional mass-market BEV registrations in Year 1. |
| 3 | Rural high-friction | Mandate pre-emptive multi-agency grid coordination MOUs in counties with Utility Complexity Score ≥ 2.5. | Reduces future infrastructure coordination cost by an estimated 30–40%. |

---

## Repository Structure

```text
SectionName_TeamID_ProjectName/
|
|-- README.md
|
|-- data/
|   |-- raw/                         # Original dataset (never edited)
|   `-- processed/                   # Cleaned output from ETL pipeline
|
|-- notebooks/
|   |-- 01_extraction.ipynb
|   |-- 02_cleaning.ipynb
|   |-- 03_eda.ipynb
|   |-- 04_statistical_analysis.ipynb
|   `-- 05_final_load_prep.ipynb
|
|-- scripts/
|   `-- etl_pipeline.py
|
|-- tableau/
|   |-- screenshots/
|   `-- dashboard_links.md
|
|-- reports/
|   |-- README.md
|   |-- project_report_template.md
|   `-- presentation_outline.md
|
|-- docs/
|   `-- data_dictionary.md
|
|-- DVA-oriented-Resume/
`-- DVA-focused-Portfolio/
```

---

## Analytical Pipeline

The project follows a structured 7-step workflow:

1. **Define** - Sector selected, problem statement scoped, mentor approval obtained.
2. **Extract** - Raw dataset sourced and committed to `data/raw/`; data dictionary drafted.
3. **Clean and Transform** - Cleaning pipeline built in `notebooks/02_cleaning.ipynb` and optionally `scripts/etl_pipeline.py`.
4. **Analyze** - EDA and statistical analysis performed in notebooks `03` and `04`.
5. **Visualize** - Interactive Tableau dashboard built and published on Tableau Public.
6. **Recommend** - 3-5 data-backed business recommendations delivered.
7. **Report** - Final project report and presentation deck completed and exported to PDF in `reports/`.

---

## Tech Stack

| Tool | Status | Purpose |
|---|---|---|
| Python + Jupyter Notebooks | Mandatory | ETL, cleaning, analysis, and KPI computation |
| Google Colab | Supported | Cloud notebook execution environment |
| Tableau Public | Mandatory | Dashboard design, publishing, and sharing |
| GitHub | Mandatory | Version control, collaboration, contribution audit |
| SQL | Optional | Initial data extraction only, if documented |

**Recommended Python libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `statsmodels`

---

## Evaluation Rubric

| Area | Marks | Focus |
|---|---|---|
| Problem Framing | 10 | Is the business question clear and well-scoped? |
| Data Quality and ETL | 15 | Is the cleaning pipeline thorough and documented? |
| Analysis Depth | 25 | Are statistical methods applied correctly with insight? |
| Dashboard and Visualization | 20 | Is the Tableau dashboard interactive and decision-relevant? |
| Business Recommendations | 20 | Are insights actionable and well-reasoned? |
| Storytelling and Clarity | 10 | Is the presentation professional and coherent? |
| **Total** | **100** | |

> Marks are awarded for analytical thinking and decision relevance, not chart quantity, visual decoration, or code length.

---

## Submission Checklist

**GitHub Repository**

- [x] Public repository created with the correct naming convention (`SectionName_TeamID_ProjectName`)
- [x] All notebooks committed in `.ipynb` format
- [x] `data/raw/` contains the original, unedited dataset
- [x] `data/processed/` contains the cleaned pipeline output
- [ ] `tableau/screenshots/` contains dashboard screenshots
- [ ] `tableau/dashboard_links.md` contains the Tableau Public URL
- [x] `docs/data_dictionary.md` is complete
- [x] `README.md` explains the project, dataset, and team
- [ ] All members have visible commits and pull requests

**Tableau Dashboard**

- [ ] Published on Tableau Public and accessible via public URL
- [ ] At least one interactive filter included
- [ ] Dashboard directly addresses the business problem

**Project Report**

- [x] Final report exported as PDF into `reports/` (Template complete)
- [x] Cover page, executive summary, sector context, problem statement
- [x] Data description, cleaning methodology, KPI framework
- [x] EDA with written insights, statistical analysis results
- [ ] Dashboard screenshots and explanation
- [x] 8-12 key insights in decision language
- [x] 3-5 actionable recommendations with impact estimates
- [x] Contribution matrix matches GitHub history

**Presentation Deck**

- [ ] Final presentation exported as PDF into `reports/`
- [ ] Title slide through recommendations, impact, limitations, and next steps

**Individual Assets**

- [ ] DVA-oriented resume updated to include this capstone
- [ ] Portfolio link or project case study added

---

## Contribution Matrix

This table must match evidence in GitHub Insights, PR history, and committed files.

| Team Member | Dataset and Sourcing | ETL and Cleaning | EDA and Analysis | Statistical Analysis | Tableau Dashboard | Report Writing | PPT and Viva |
|---|---|---|---|---|---|---|---|
| **Omved Nagre** | Owner | Support | Owner | Owner | Support | Owner | Support |
| **Om Mishra** | Owner | Support | Support | Support | Support | Support | Support |
| **Gogulamudi JayaDeep** | Support | Owner | Support | Support | Support | Support | Support |
| **Sushant** | Support | Support | Owner | Owner | Support | Support | Support |
| **Harsh Raj** | Support | Support | Support | Support | Owner | Support | Support |
| **Anshuman Mehta** | Support | Support | Support | Support | Support | Support | Owner |

_Declaration: We confirm that the above contribution details are accurate and verifiable through GitHub Insights, PR history, and submitted artifacts._

**Team Lead Name:** `Omved Nagre`

**Date:** `April 2026`

---

## Academic Integrity

All analysis, code, and recommendations in this repository must be the original work of the team listed above. Free-riding is tracked via GitHub Insights and pull request history. Any mismatch between the contribution matrix and actual commit history may result in individual grade adjustments.

---

*Newton School of Technology - Data Visualization & Analytics | Capstone 2*
