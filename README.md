# 🔥 Forest Fire Analysis & Prediction

> **Data Analytics Capstone Project --- The Knowledge House**

<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/e4f5fc39-3558-4cbb-bcf6-fb45335da173" />


This machine learning project explores forest fire activity in **Montesinho Natural Park,
Portugal**, using:

- Exploratory Data Analysis 

- Preprocessing 

- Fire Weather Index variables

- K-Means clustering

- Regression Modeling

- Tableau visualization

- Final Report

Our central question was:

**Can weather conditions, fire-weather indicators, location, and
seasonal patterns help us understand forest fire activity and burned
area?**

------------------------------------------------------------------------
🎯 Intended Audience
This analysis is intended for **forest management agencies** **park rangers**, and **regional fire risk planners** who need to prioritize monitoring resources and staffing during the fire season. It may also be useful to **environmental researchers** and **insurers** evaluating how seasonal weather conditions relate to fire risk in similar Mediterranean climate zones. The findings help answer a practical question for these stakeholders: given current weather and fuel conditions, how much elevated attention does a given period or location warrant?

------------------------------------------------------------------------

## 📊 Dashboard     
![Forest Fire Risk & Environmental Conditions Dashboard](Dashboard/Final_Dashboard.png)

### At a Glance

-   🔥 Fire activity peaks in **August and September**
-   📈 September records the greatest total burned area
-   🌡️ Peak fire months are dominated by **hotter and drier conditions**
-   🧩 K-Means found **3 main environmental clusters**
-   🎯 Best clustering result: **Cluster 1 from K = 3 - Hot Dry High Risk Group**
-   📉 Best regression model (G) combined the FWI system with the cluster label, reaching R² = 0.050 modest, but the only configuration to meaningfully outperform a mean prediction         baseline
-   ⚠️ Weather and fire-weather indicators provide useful context, but
    no single variable strongly explains exact burned area

------------------------------------------------------------------------

## 👥 Team
**Name | Role | Contributions**
------------- ------------- -------------------
Palo Becerra | Data Analyst | Team Lead - facilitated group discussions to consolidate findings into a shared analysis plan, conducted independent EDA, designed and built the final Tableau dashboard, authored the Final Report, and contributed to the project repo as needed.

Lofinda Beynis | Data Analyst | Repo Lead - Set up Repo, Did EDA, dashboarding and an unsupervised learning model

Amir Benston | Data Analyst | Model Lead - Led exploratory data analysis, engineered the Fire Weather Index (FWI) and Buildup Index (BUI) features, built and evaluated eight supervised regression models (Linear Regression and Random Forest) comparing raw weather variables against composite fire danger indices, and integrated the unsupervised K-Means cluster label into the final, best performing model.

------------------------------------------------------------------------

## 📁 Dataset

The project uses the **Forest Fires Dataset** from the UCI Machine
Learning Repository.

The raw dataset contains **517 observations and 13 variables**
describing spatial location, calendar information, weather conditions, Fire Weather Index components, and burned area.

------------------------------------------------------------------------

### Main Variables

  Variable     Description
  ------------ -------------------------------
  X, Y         Spatial coordinates
  
  month, day   Calendar information
  
  FFMC         Fine Fuel Moisture Code
  
  DMC          Duff Moisture Code
  
  DC           Drought Code
  
  ISI          Initial Spread Index
  
  temp         Temperature (°C)
  
  RH           Relative humidity (%)
  
  wind         Wind speed
  
  rain         Rainfall
  
  area         Forest area burned (hectares)
  
  log_area     Log transformed area - Engineered Feature
  
  FWI          Fire Weather Index - Engineered Feature
  
  BUI          Buildup Index - Engineered Feature

------------------------------------------------------------------------

## 🔎 Exploratory Data Analysis

EDA was used to understand the structure of the data, identify seasonal
patterns, examine burned area behavior, and explore relationships
between environmental variables.

### Data Quality

-   **517** original observations
-   **0 missing values**
-   **4 exact duplicate rows**
-   **513 observations** remained after duplicate removal

### Burned Area

Burned area is highly right-skewed. Most fires burned relatively small
areas, while a small number of observations represent much larger fires.

**247 of the 517 raw observations (47.8%) report 0 hectares burned.**

To reduce the effect of extreme values, we created:

`log_area = log(1 + area)`

### Seasonal Patterns

The strongest descriptive pattern is the concentration of forest fire
activity in late summer.

-   **August:** 184 observations
-   **September:** 172 observations
-   **September:** greatest total burned area

This distinction is important: the month with many fires is not
necessarily the month with the same pattern of fire severity.

### Correlation Findings

No individual numerical feature showed a strong linear relationship with
burned area.

The EDA therefore suggests that **fire severity cannot be explained by
one weather or Fire Weather Index variable alone**.

------------------------------------------------------------------------

## ⚙️ Data Preprocessing

The preprocessing stage prepared the data for feature engineering, clustering, and regression modeling.

Key steps included:

- Log transforming the target: `log_area = log(1 + area)`
- Deriving BUI and FWI from ISI, DMC, and DC (see Fire Weather Index Analysis below)
- Mapping `month` and `day` to numeric codes for use in modeling
- Standardizing clustering features (`FFMC`, `BUI`, `FWI`, `temp`, `RH`, `wind`) with `StandardScaler` ahead of K-Means

This pipeline is the one that fed the final K-Means clustering, regression modeling, and Tableau dashboard presented in this README.

> **Note:** A separate preprocessing and clustering pipeline (one-hot encoding, `Lofinda_preprocessing.ipynb` and `Lofinda_unsupervised_learning.ipynb`) was also explored by the team. The pipeline described above is the one used for the final presentation, dashboard, and regression modeling results in this README.

------------------------------------------------------------------------

## 🌡️ Fire Weather Index Analysis

FWI and BUI were derived directly from the raw fuel moisture codes (FFMC, DMC, DC) and the Initial Spread Index (ISI), following the standard three-tier FWI system calculation rather than relying on a pre existing column.

The analysis explores how these composite indicators relate to forest fire behavior. While they help characterize environmental fire conditions more effectively than raw weather variables alone (see Regression Modeling below), **no single
feature is a strong standalone explanation of burned area**.

File:

`Data/processed/Amir_forestfires_with_fwi.csv`

------------------------------------------------------------------------

## 🧩 Unsupervised Learning --- K-Means

K-Means clustering was used to determine whether forest fire
observations naturally separate into meaningful environmental groups.

### Clustering Features

Seven standardized variables were used:

-   FFMC
-   BUI
-   FWI
-   Temperature
-   Relative humidity
-   Wind

Burned area was intentionally **not used to create the clusters**.

### Why Rain Was Excluded

Rain was zero in **505 of 513 observations (98.4%)**.

Because K-Means relies on distance, the few non-zero rain observations
could disproportionately affect the clustering solution. Rain was
therefore excluded from the final clustering features.

### Selecting the Number of Clusters

Solutions from **K = 1 through K = 7** were compared using the elbow method.

**Best solution:** K = 3. The Steepest drop is between k = 1 and 2. Every single drop after is less steep and flattens out at K = 3 which is what we choose for the number of clusters.

### Cluster Interpretation
**Cluster 0 --- Low Risk Category**

Cool temperatures (12.5°C), moderate humidity (45.2%), and the lowest fuel buildup of the three groups (BUI=37.4, FWI=10.4). Containment is the typical outcome here

**Cluster 1 --- Hot Dry High Risk Category**

The dominant cluster, comprising 58% of records. Characterized by high temperatures (22.3°C), the lowest relative humidity (36.6%), and by far the highest fuel buildup (BUI=167.8) and composite fire danger (FWI=37.1). This is the only cluster where measurable fire spread is the *typical* outcome rather than the exception.

**Cluster 2 --- High Fuel, Humidity Contained Category**

 Fuel buildup and fire danger are nearly as high as Cluster 1 (BUI=183.6, FWI=34.3), but relative humidity is nearly double (65.6% vs. 36.6%) and temperatures are notably cooler (16.0°C). Despite comparable fuel and danger scores to Cluster 1, containment not spread is the typical outcome here 

**Key takeaway:** Comparing Clusters 1 and 2 both have similarly high fuel buildup and composite fire danger scores, yet they differ greatly in typical outcome; spread is typical in Cluster 1 (median 1.04) but not in Cluster 2 (median 0.00). The main distinguishing factor between them is humidity (36.6% vs. 65.6%), suggesting humidity plays a substantial mediating role even when fuel accumulation and danger indices are both high. This interaction is not visible in the univariate correlation heatmap, where RH's linear correlation with area alone was weak (~-0.08).

------------------------------------------------------------------------

📉 Supervised Learning --- Regression Modeling
To test whether burned area (modeled as log_area) could be predicted from environmental conditions, eight model configurations were built and compared, ranging from linear regression to Random Forest models with different feature sets. Each was trained on an 80/20 train-test split, using MAE, RMSE (on the log scale), and R² as evaluation metrics.

Model Comparison
Model Description Algorithm MAE (area) RMSE (log) R²
------- ------------------------------------------------ --------------- ------------ ------------ --------
A FWI + temp + FFMC + RH Linear 19.832 1.483 -0.001
B FWI + FFMC + RH + wind Linear 19.808 1.475 0.011
C FFMC + DMC + DC + temp Random Forest 19.681 1.497 -0.020
D temp + ISI + BUI + FWI Random Forest 19.979 1.522 -0.054
E Full fire-index set + temp Random Forest 19.752 1.496 -0.018
fwi_compile Full FWI chain, no raw weather Random Forest 19.450 1.452 0.041
F fwi_compile + wind + rain Random Forest 19.747 1.464 0.024
G fwi_compile + K-Means cluster label (best model) Random Forest 19.422 1.440 0.050

Key Findings
Raw weather variables consistently hurt performance. Adding temp to the fire index chain (Model E) dropped R² from 0.041 to -0.018; adding wind and rain (Model F) dropped it to 0.024. The full FWI system already captures the weather signal that matters layering raw weather back on top adds noise rather than help.

Random Forest did not outperform Linear Regression on raw features. Models C, D, and E all returned negative R², underperforming even the weak but positive linear baseline (Model B, R² = 0.011). Increasing model flexibility did not compensate for limited signal in the underlying data.

The unsupervised cluster label was the single addition that improved performance. Model G the same FWI based feature set as the best baseline, plus the K-Means cluster assignment was the only configuration across the entire comparison to improve on its base model rather than degrade it, reaching the best overall R² of 0.050.

Predicted vs. actual results show a consistent weakness across all models: even the best model (G) struggles to catch the rare, large fires the largest actual burned area (log_area ≈ 7.0) was predicted at only ≈ 3.3 and it also overestimates the many near zero burned area days.

Interpretation
Across all eight models, R² ranged from -0.054 to 0.050, meaning even the best performing model explains only about 5% of the variance in burned area. This is consistent with the weak univariate correlations found during EDA and reflects the well documented difficulty of this dataset. The clearest actionable finding is that composite fire danger indices (the FWI system) outperform raw weather variables as predictors, and that unsupervised clustering on weather/fuel conditions adds a small but consistent improvement beyond any individual raw or composite feature alone.

------------------------------------------------------------------------

## 📈 Tableau Analysis

### Predicted Burned Area in Northeast Portugal

![Forest Fire Analysis: Predicted Burned Area in Northeast Portugal](Dashboard/Final_Dashboard.png)

This dashboard was the primary visual used in our final presentation. It brings together the EDA, clustering, and regression modeling results into a single view, organized around our central question: **the purpose of this study was to build a model that could predict what caused some fires to burn more area than others.**

It includes:

- **Correlation heatmap** — no single weather variable or fire index correlated strongly with burned area; the strongest was temperature at only 0.10
- **Cluster median burned area** — Cluster 1 stood alone as the only group where measurable fire spread was the typical outcome (median 1.09 ha), versus 0.00 in the other two clusters
- **Forest fires by month** — August and September account for the large majority of fire activity, consistent with our EDA findings
- **Burned area by month** — September posted the highest total burned area (3,086 ha), despite August having more individual fires
- **Regression model comparison (Models A–G)** — visualizes the R² of all eight models tested; every raw feature model scored at or below zero, and **Model G**, which added the K-Means cluster label to the FWI feature set, was the only model to post a meaningfully positive R²
- **FFMC vs. log(area + 1)** — of all raw variables tested, the FFMC composite index showed the clearest (though still weak) relationship with burned area
- **Cluster feature profiles (K = 3)** — average FWI, relative humidity, and temperature by cluster, showing the same Hot/Dry vs. Humid/Contained pattern found in the EDA
- **Predicted vs. actual (Model G)** — visualizes our best model's performance; even Model G consistently under predicted the largest fires

### Dashboard Findings

The dashboard reinforces the central finding across the entire project: **the FWI system outperformed raw weather variables as a predictor, and even after applying K-Means clusters to our best model (Model G), we were only able to build a weak regression model.** No single variable, index, or model configuration reliably predicted fire size; grouped risk clusters came closest, but even that improvement was modest.

------------------------------------------------------------------------

## 💡 What We Learned

### Environmental Insight

Forest fire activity in this dataset is strongly seasonal. **August and
September** account for most observations, and hotter/drier
environmental conditions are especially common during this period.

### Analytical Insight

Environmental variables are useful for identifying **fire-weather
patterns**, but they are much less effective at explaining the exact
amount of land burned.

### Machine Learning Insight

K-Means successfully separates the observations into interpretable environmental groups, and integrating that cluster label into a regression model produced the best performing predictor of burned area tested (R² = 0.050) though substantial overlap in burned area outcomes remains between clusters, and no model reliably predicts the rare, large fires.

## Main Limitation

The available dataset captures important weather and fire danger information, but forest fire severity is complex. Factors not represented in the dataset such as fuel type, ignition cause, terrain, or firefighting response time — may also influence how large an individual fire becomes.

------------------------------------------------------------------------

🎯 Recommendations
- Prioritize monitoring resources during August and September, and specifically during periods matching the Cluster 1 profile (temperatures above ~20°C, relative humidity below ~40%), since this is the only environmental profile where measurable fire spread is the typical outcome rather than the exception.

- Track humidity as a leading indicator, not just temperature or fuel buildup; Clusters 1 and 2 show nearly identical fuel and danger scores but sharply different outcomes, with humidity as the main distinguishing factor.

- Use the composite FWI system, not raw weather readings, as the basis for any operational risk scoring, since raw weather variables consistently degraded model performance relative to the derived fire danger indices.

- Treat exact burned area predictions with caution. Given the best model explains only ~5% of variance, use these models for relative risk categorization (e.g., "elevated risk period") rather than for forecasting a specific expected burned area.

------------------------------------------------------------------------

🔭 Limitations & Next Steps
**Limitations:**

- No single environmental variable or model configuration strongly predicts exact burned area (best R² = 0.050).

- The dataset lacks fuel-type, ignition-cause, terrain, and response-time variables that likely influence fire severity.

- The test set is relatively small (n ≈ 104), so model comparisons should be interpreted cautiously.

**Next Steps:**

- Test gradient boosting methods (e.g., XGBoost, LightGBM) to see whether they capture non linear interactions better than Random Forest did here.

- Incorporate additional data sources, such as satellite derived vegetation indices or historical ignition records, if available.

- Explore classification instead of regression (e.g., "fire vs. no fire" or risk tier) given how difficult exact burned area prediction proved to be.

------------------------------------------------------------------------

📄 Final Report
The full written report, including detailed methodology and additional analysis beyond this README, is available at:

`Reports/Final_Report.pdf`

------------------------------------------------------------------------

## 🗂️ Repository Structure

``` text
TEPP_CAPSTONE_PROJECT/
│
├── Dashboard/
│   ├── Final_Dashboard.png
│   ├── forest_fire_dashboard_LB.png
│   └── forestfires_tableau_LB.csv
│
├── Data/
│   ├── processed/
│   │   ├── Amir_forestfires_with_fwi.csv
│   │   ├── Lofinda_forestfires_processed.csv
│   │   └── Lofinda_forestfires_with_clusters.csv
│   └── raw/
│       └── forestfires.csv
│
├── Docs & Resources/
│   ├── Data Dictionary
│   ├── forestfires.names
│   ├── Requirement.md
│   ├── Resources.md
│   └── Figures/
│       ├── 01_area_distribution.png
│       ├── 02_feature_histograms.png
│       ├── 03_weather_vs_fwi.png
│       ├── 04_correlation_heatmap.png
│       ├── 05_elbow_method.png
│       ├── 06_predicted_vs_actual.png
│       ├── 07_FWI flowchart.png
│       ├── 08_FWI rating system.png
│       └── 09_Montesinho_Sunrise.png
│
├── Notebooks/
│   ├── amir_fire_severity_modeling.ipynb
│   ├── Lofinda_eda.ipynb
│   ├── Palo_eda.ipynb
│   ├── Lofinda_preprocessing.ipynb
│   └── Lofinda_unsupervised_learning.ipynb
│
├── Reports/
│   └── Final_Report.pdf
│
├── .gitignore
└── README.md

------------------------------------------------------------------------

## 🛠️ Tools & Technologies

**Python** • **Pandas** • **NumPy** • **Matplotlib** • **Seaborn** •
**Scikit-learn** • **Jupyter Notebook** • **Tableau** • **Git** •
**GitHub** • **VS Code**

------------------------------------------------------------------------

## 🎯 Conclusion

This project combined exploratory data analysis, preprocessing, Fire
Weather Index analysis, K-Means clustering, regression analysis, and
Tableau visualization to better understand forest fire behavior.

The analysis identified a strong seasonal pattern, with fire activity
concentrated in **August and September**. K-Means also revealed
distinct environmental profiles, particularly hotter/drier and
cooler/more-humid conditions, and integrating that cluster label into
a regression model (Model G) produced the best performing predictor
tested across eight configurations, though it still explained only
about 5% of the variance in burned area (R² = 0.050).

However, the correlation and regression analyses showed that
**predicting exact burned area remains challenging**. The available
weather and fire weather variables provide useful information about the
conditions associated with forest fires, but they have limited ability
to explain the severity of an individual fire.

Overall, the project shows that environmental data is valuable for
identifying **when and under what conditions fire activity is more
common**, while accurately predicting **how much area will burn**
requires additional information and potentially more advanced data or
modeling approaches.
