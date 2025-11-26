# Turbulent Politics: How storms do or don't affect voting behavior in presidential elections
Investigating changes in county-level voting during presidential elections using aggregate crop/property damage and deaths caused by storms leading up to voting

## Group Members
Sumant Sharma
Ethan Greenberg

---

## Overview
Given the political controversy around climate change, this project explores if natural disasters correlate with shifts in presidential voting behavior from 2000 to 2024. Using county-level storm data from NOAA and election results from the MIT Election Data Lab, we analyze the relationship between storm severity and changes in Democratic and incumbent vote percentages. Our analysis includes exploratory scatter plots, tornado-specific intensity analysis, and predictive modeling using Random Forest Regression with Shapley Additive Explanations (SHAP) to assess feature importance.

---

## Hypothesis
In the wake of natural disasters, districts that were most affected will be more likely to have a regional shift towards the democratic party as a result of increased awareness of climate change. We also expect that there will be a general sentiment against incumbents, as people will be upset and the incumbent will be the victim of misdirected anger.

---

## Data

### Storm Data
- **Source:** [NOAA Storm Events Database](https://www.ncdc.noaa.gov/stormevents/)
- **Years:** 2000–2024
- **Preprocessing:**
  - Converted crop and property damage strings (e.g., `100M`, `10K`) to integer values.
  - Filtered for county-level events (`CZ_TYPE == "C"`) only.
  - Created `county_fips_cleaned` (5-digit FIPS code) as the merge key.
  - Aggregated data into election quads (e.g., Nov 2004–Nov 2008 for the 2008 election).

### Election Data
- **Source:** [MIT Election Data Lab](https://electionlab.mit.edu/data)
- **Coverage:** County-level presidential election results, 2000–2024
- **Preprocessing:**
  - Standardized `county_fips` as a 5-digit column.
  - Calculated change in Democratic vote % and incumbent vote % between consecutive elections.

---

## Directory
```

├── Figures/ # Plots and visualizations
│ ├── AggCropDamage_Versus_Change_Democratic_votepercent.png
│ ├── AggCroppDamage_Versus_Change_Incumbent_votepercent.png
│ ├── AggDeaths_Versus_Change_Democratic_votepercent.png
│ ├── AggDeaths_Versus_Change_Incumbent_votepercent.png
│ ├── AggPropDamage_Versus_Change_Democratic_votepercent.png
│ ├── AggPropDamage_Versus_Change_Incumbent_votepercent.png
│ ├── TornadoStrength_Versus_AvgChange_Democratic_votepercent.png
│ ├── damage_distribution.png
│ ├── model2_SHAP_plot.png
│ ├── model3_SHAP_plot.png
│ ├── model4_SHAP_plot.png
│ └── zoom_in_on_2012.png
├── NWS_Correlation_FIle/ # Files for NOAA forecast zone to county correlation
│ └── nws_county_correlation.dbx
├── election_data/ # MIT Election Data Lab files
│ └── countypres_2000-2024.csv
├── storm_data/ # NOAA storm data CSVs and documentation
│ ├── Storm-Data-Bulk-csv-Format.pdf
│ ├── StormEvents_details-ftp_v1.0_d2000_c20250520.csv
│ ├── StormEvents_details-ftp_v1.0_d2001_c20250520.csv
│ └── ... (2002–2024)
├── QSS20Final.ipynb # Jupyter Notebook with preprocessing, analysis, and modeling
└── README.md # This file
```

## Notes on Data Storage and Processing

We are analyzing U.S. county-level storm events and their potential influence on presidential election voting behavior from 2000 to 2024. In the GitHub, we provide preprocessed files and visualizations for ease of analysis and reproducibility. While raw storm data CSVs are provided, the full dataset can also be obtained externally from the NOAA Storm Events database above.  

We also attempted to construct a forecast-zone-to-county crosswalk for hurricane coverage using the file
/NWS_Correlation_File/nws_county_correlation.dbx.
After working on in though, we were not able to integrate this in a way that would be meaningfully usable analysis on county-level hurricane aggregation, and we ultimately did not use it in the final analysis. 

---

## Methodology

1. **Exploratory Analysis**
   - Plotted changes in Democratic/incumbent vote % against aggregate storm variables (property damage, crop damage, deaths).
   - Smoothed curves using generalized additive models (`geom_smooth` in plotnine).

2. **Tornado Intensity Analysis**
   - Calculated average change in Democratic vote % by tornado strength for each election quad.

3. **Predictive Modeling**
   - Built four Random Forest Regression models:
     1. Aggregate storm variables
     2. Aggregate variables + state dummies
     3. Time-split (past elections → future elections)
     4. Time-split + state dummies
   - SHAP analysis performed on models 2–4 to determine feature influence.

---

## Results
- No clear correlation was observed between storm severity and voting behavior in any election quad.
- Tornado strength did not significantly affect Democratic vote % changes.
- Random Forest models had negative or near-zero R² values and high MAEs, indicating no predictive power.
- SHAP analysis confirmed that storm-related features did not systematically influence predictions
