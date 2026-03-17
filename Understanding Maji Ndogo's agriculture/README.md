# Maji Ndogo Agriculture Analysis Project

**Project Title:** Understanding Agricultural Patterns in Maji Ndogo  
**Objective:** Analyze environmental, soil, and management factors to identify optimal conditions for crop cultivation and support data-driven farming decisions.  
**Dataset Source:** SQLite database (`Maji_Ndogo_farm_survey.db`) containing geographic, weather, soil & crop, and farm management features.  
**Tools Used:** Python, Pandas, SQLAlchemy

## Project Overview

The goal of this project is to explore relationships between environmental conditions (elevation, rainfall, temperature), soil characteristics, pollution levels, and crop performance (standardized yield). The analysis helps determine:

- Which crops perform best under specific conditions
- Where the most fertile soils are located
- Ideal climate and pollution profiles for high-yielding fields

## Data Preparation

### Database Integration
- Connected to SQLite database using SQLAlchemy
- Joined four tables on `Field_ID`:
  - `geographic_features`
  - `weather_features`
  - `soil_and_crop_features`
  - `farm_management_features`

### Data Cleaning Steps
- Renamed `Chosen_crop` → `Crop_type` (to match analysis expectations)
- Converted negative `Elevation` values to positive using `.abs()`
- Standardized `Crop_type` values:
  - Converted to lowercase and stripped whitespace
  - Corrected typos: `cassaval` → `cassava`, `teaa` → `tea`, `wheatn` → `wheat`, etc.
- Ensured `Annual_yield` is numeric (`float64`)

**Final unique crop types (after cleaning):** 8  
(cassava, tea, wheat, potato, banana, coffee, rice, maize)

## Key Analyses & Challenges

### 1. Crop Distribution by Climate
**Function:** `explore_crop_distribution(df, crop_filter)`  
**Purpose:** Returns mean rainfall and elevation for a specified crop.

**Example results:**
- Tea: ≈ (1534.5 mm rainfall, 775.2 m elevation)
- Wheat: ≈ (1010.3 mm rainfall, 595.8 m elevation)

### 2. Soil Fertility by Type
**Function:** `analyse_soil_fertility(df)`  
**Purpose:** Groups data by `Soil_type` and computes average `Soil_fertility`.

**Key insight:**  
Silt and Volcanic soils show the highest average fertility (~0.65), while Rocky soils are the least fertile (~0.58).

### 3. Climate & Geography Influence
**Function:** `climate_geography_influence(df, column)`  
**Purpose:** Groups by specified column (e.g. `Crop_type`) and returns mean values for:
- Elevation
- Min_temperature_C
- Max_temperature_C
- Rainfall

**Notable patterns:**
- Tea grows at highest average elevation (~775 m)
- Rice grows at lowest elevation (~353 m) with highest rainfall (~1632 mm)
- Banana requires the highest rainfall (~1660 mm)

### 4. Identifying Top-Performing Crop
**Function:** `find_ideal_fields(df)`  
**Purpose:** Finds the crop with the highest number of above-average `Standard_yield` fields.

**Result:**  
Maize (or Cassava / Tea – depending on exact dataset version) most frequently achieves above-average performance.

### 5. Optimal Conditions for High Performers
**Function:** `find_good_conditions(df, crop_type)`  
**Filters applied:**
- Selected crop type
- `Standard_yield` > dataset average
- `Ave_temps` between 12 °C and 15 °C (inclusive)
- `Pollution_level` < 0.0001

**Example result (tea):**  
≈ 14 fields meet all strict high-performance + clean + moderate temperature conditions.

## Summary of Findings

- **High-rainfall crops** (tea, banana, rice, coffee) thrive at different elevations and temperature ranges.
- **Silt and Volcanic soils** offer the best natural fertility.
- **Low-pollution, moderate-temperature fields** (12–15 °C) with above-average yields are rare but valuable.
- **Maize** frequently appears among top performers, suggesting robustness across conditions.

## Next Steps

- Build predictive models for crop suitability (classification / regression)
- Visualize spatial distribution using latitude/longitude
- Recommend deployment locations for precision agriculture technology
- Incorporate time-series weather forecasts for planning

---

**Prepared by:** [Daniel Nzioki Musyoka]  
**Date:** March 2025 – March 2026  
**Course/Module:** Integrated Pandas & Data Analysis Project