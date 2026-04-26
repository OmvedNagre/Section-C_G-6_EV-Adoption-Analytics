# Data Cleaning & Feature Engineering Insights

## 1. Project Overview
This document details the ETL (Extract, Transform, Load) process for the Washington State Electric Vehicle Population dataset. The goal was to transform a raw administrative registry into a strategically segmented dataset optimized for policy analysis and Tableau visualization.

## 2. Initial Data Audit
**Raw Row Count**: 280,833

**Missing Values Found**: Minor gaps in geographic data (County, City, Legislative District) and significant gaps in recorded battery performance.

**Data Quality Issues**: Postal Code imported as a float (e.g., 98034.0), and Electric Range contained a high volume of 0 values, which represented "Not Researched" rather than a zero-mile range.

## 3. Standardization & Cleaning Steps
**Geographic Imputation**: All missing geographic fields were filled with "Unknown" to prevent row loss during grouping.

**Postal Code Normalization**: Converted to string type to remove decimal artifacts, ensuring correct mapping in Tableau.

**The "0 Range" Decision**: Replaced 0 with NaN in the primary analysis column to prevent skewed averages, while maintaining Electric Range Raw for total population counts.

## 4. Strategic Feature Engineering
Five new features were engineered to provide analysis depth:

**Geospatial Engineering (Lat/Long)**:
- Logic: Extracted numeric coordinates from the Vehicle Location POINT strings.
- Purpose: Enables high-resolution heatmaps and density plots beyond simple county borders.

**Market Segmentation (Market_Segment)**:
- Logic: Classified high-premium manufacturers (Tesla, Lucid, Rivian, etc.) as "Luxury/Premium" and others as "Mass Market."
- Purpose: Evaluates market accessibility and the effectiveness of incentives for different socio-economic groups.

**Eligibility Traffic Lights (Eligibility_Status)**:
- Logic: Simplified long text strings into a 3-tier status: Eligible, Not Eligible, and Unknown.
- Purpose: Improves dashboard readability and streamlines KPI filtering.

**Adoption Waves (Adoption_Wave)**:
- Logic: Binned Model Year into three eras: Early Adopter (<=2015), Market Growth (2016-2020), and Mass Adoption (2021-Present).
- Purpose: Visualizes the velocity of the EV transition over time.

**Utility Complexity Score (Utility_Complexity_Score)**:
- Logic: Counted the number of utility jurisdictions listed per registration (e.g., PUGET SOUND ENERGY INC||CITY OF TACOMA = 2).
- Purpose: Acts as a proxy for Administrative Friction. Higher scores indicate regions where policy implementation (like grid upgrades) requires multi-agency coordination.

## 5. Data Filtering for Range Accuracy
To ensure the mathematical validity of our battery technology analysis, a subset was created focusing on Verified Range Data:

**Decision**: Rows where battery range was not researched (0 or NaN) were filtered out for range-specific KPIs.

**Impact**: While the total population registry remains at ~280k for volume counts, performance analysis is based on a high-quality subset of ~101k vehicles.

## 6. Final Export Specifications
**Format**: CSV

**Filename**: EV_Final_Ready_Tableau.csv

**Output**: 26 Columns, including all engineered features and standardized geographic data.