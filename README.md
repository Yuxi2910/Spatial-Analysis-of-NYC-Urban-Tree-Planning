# Spatial Analysis of NYC Urban Tree Planning  
**Green Space and Socioeconomic Factors: A Spatial Analysis of NYC's Urban Tree Planting**

This project examines how **street tree density** varies across New York City census tracts and how it relates to **population density, median household income, and racial/ethnic composition (Asian, White, Black)**. The goal is to identify spatial patterns and potential inequities in access to urban greenery, with implications for **urban planning and environmental justice**.

---

## Research Questions
- How does tree density vary across NYC neighborhoods with different population densities?
- Is there a relationship between income levels and tree density?
- Do higher-income or less densely populated areas have more trees, and to what extent?
- Which ethnic group has the highest share living in high tree-density areas?

---

## Hypotheses
1. **Tree density is negatively correlated with population density.**
2. **Higher-income neighborhoods have higher tree density.**
3. **Neighborhoods with a higher proportion of White residents have greater tree density than those with higher proportions of Black or Asian residents.**

---

## Data Sources
This project integrates three core datasets:

1. **NYC Street Tree Census (2015)**  
   - Point data of street trees across NYC (location, diameter, status, etc.).  
   - Only **living trees** are retained; **dead** and **stump** records are excluded.  
   - Source: NYC Open Data  
     https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/pi5s-9p35

2. **NYC Census Tracts Boundary Shapefile**  
   - Tract boundaries used as the unit of analysis.  
   - Source: US Census Bureau TIGER/Line  
     https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html

3. **Demographic & Socioeconomic Data (2020)**  
   - Total population, median household income, and racial/ethnic counts.  
   - Used to compute population density and racial/ethnic composition rates.  
   - Source: US Census Bureau / ACS  
     https://www.census.gov/programs-surveys/acs

> **Note:** Tree data (2015) and demographic data (2020) are not from the same year, which may introduce bias.

---

## Methods & Workflow

### Spatial Processing (QGIS)
Key steps performed in QGIS include:
- Load NYC census tract polygons (`nyct2020.shp`).
- Load tree point dataset (2015).
- Filter to keep **alive** trees.
- **Join attributes by location** to attach tree points to tracts.
- Join tract polygons with demographic CSV using **GEOID**.
- Use **Count Points in Polygon** to count living trees per tract.
- Compute:
  - **Tree density** = (living tree count / tract area) × 100  
  - **Ethnic composition (%)** = (group population / total population) × 100

### Spatial Statistics (GeoDa)
GeoDa is used for:
- **Global spatial autocorrelation** (Moran’s I)
- **LISA cluster maps**
- Regression models:
  - **OLS**
  - **Spatial Lag Model (SLM)**
  - **Spatial Error Model (SEM)**

---

## Key Results (High-Level)
- **Moran’s I ≈ 0.464**, suggesting moderate-to-strong positive spatial clustering of tree density.
- The **OLS model** shows limited explanatory power (**R² ≈ 0.081**).
- Spatial models significantly improve fit:
  - **SLM R² ≈ 0.467**
  - **SEM R² ≈ 0.477**
- **Income** shows a positive association with tree density.
- **Racial composition** variables are overall negatively associated with tree density, with the **White population showing the weakest negative association** and the **Black population the strongest**, aligning with Hypothesis 3.
- The relationship between **population density and tree density** appears **positive** in this analysis, which does **not support Hypothesis 1**.

---

## Repository Structure

This repo includes cleaned data, analysis outputs, and project files:

- `README.md`  
  Project overview and instructions.

- `NYC Tree research project.pdf`  
  Full write-up including maps, regression outputs, and discussion.

- `Demographic dataset.csv` / `Demographic dataset.numbers`  
  Processed tract-level demographic and socioeconomic variables.

- `Statistic tables.numbers`  
  Regression and summary outputs formatted for reporting.

- `nyct2020.shp`  
  NYC census tract boundary shapefile.

- `new Edited Tree project.qgz`  
  QGIS project file with maps and layers.

- `Tree availability distribution map.gda`  
  GeoDa project file for spatial stats and regressions.

---

## Reproducibility Guide

### Requirements
- **QGIS** (3.x recommended)
- **GeoDa**
- Optional: Excel/Numbers for reviewing tables

### Steps
1. **Download raw tree data** from NYC Open Data (link above).  
2. Open QGIS project:
   - `new Edited Tree project.qgz`
3. Ensure layers load correctly:
   - `nyct2020.shp`
   - 2015 tree point layer (downloaded)
   - `Demographic dataset.csv`
4. Apply filters to tree status:
   - Keep only **alive** entries.
5. Run:
   - **Join attributes by location**
   - **Join by GEOID** with demographic data
6. Use **Count Points in Polygon** to generate tract-level tree counts.
7. Calculate tree density fields.
8. Export the final tract-level dataset if needed.
9. Open GeoDa project:
   - `Tree availability distribution map.gda`
10. Run:
   - Moran’s I
   - LISA
   - OLS / SLM / SEM

---

## Limitations & Future Work
- **Year mismatch** (2015 tree census vs. 2020 demographics) may affect causal interpretation.
- **Tree density** could be replaced or complemented by **tree canopy cover (TCC)** to better reflect ecological benefits.
- Future versions could include additional social indicators (education, age structure) and historical policy context (e.g., redlining).

---

## Project Context
This project was completed for:  
**QMSS GR5070 – GIS & Spatial Analysis (Social Sciences)**  
Columbia University

