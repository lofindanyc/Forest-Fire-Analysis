# 🔥 Forest Fire Analysis & Prediction

## Overview

This capstone project analyzes forest fire data from the **Montesinho Natural Park in Portugal** to better understand how weather conditions and fire-related environmental indicators are associated with forest fire activity and burned area.

The project brings together the main skills developed during The Knowledge House Data Analytics Fellowship, including **data cleaning, exploratory data analysis (EDA), preprocessing, unsupervised learning, supervised machine learning, and dashboarding**.

Our goal is to identify meaningful patterns in forest fire data and explore whether environmental and weather conditions can help explain or predict the severity of forest fires.

---

## Team

| Name    | Role                            |
| ------- | ------------------------------- |
| Palo    | Data Analyst                    |
| Lofinda | Data Analyst                    |
| Amir    | Data Analyst                    |

---

## Dataset

This project uses the **Forest Fires Dataset** from the UCI Machine Learning Repository.

The dataset contains forest fire observations from the **Montesinho Natural Park in northeastern Portugal** and includes weather conditions, Fire Weather Index indicators, spatial information, and the amount of forest area burned.

### Main Variables

| Variable | Description               |
| -------- | ------------------------- |
| X        | X-axis spatial coordinate |
| Y        | Y-axis spatial coordinate |
| month    | Month of the year         |
| day      | Day of the week           |
| FFMC     | Fine Fuel Moisture Code   |
| DMC      | Duff Moisture Code        |
| DC       | Drought Code              |
| ISI      | Initial Spread Index      |
| temp     | Temperature in °C         |
| RH       | Relative humidity (%)     |
| wind     | Wind speed                |
| rain     | Rainfall                  |
| area     | Forest area burned        |

---

## Project Workflow

### 1. Data Cleaning

The dataset is inspected and cleaned before analysis.

This includes:

* Checking for missing values
* Checking for duplicate records
* Reviewing data types
* Examining categorical and numerical variables
* Identifying potential outliers
* Preparing the dataset for further analysis

---

### 2. Exploratory Data Analysis (EDA)

Exploratory Data Analysis is used to better understand the dataset and identify important patterns and relationships.

The EDA includes:

* Distribution of burned area
* Temperature distribution
* Relative humidity analysis
* Wind and rainfall analysis
* Fire Weather Index analysis
* FFMC, DMC, DC, and ISI distributions
* Correlation analysis
* Scatterplots comparing fire-weather indicators
* Relationships between environmental conditions and burned area

---

### 3. Data Preprocessing

The data is prepared for machine learning by transforming the original dataset into a format suitable for modeling.

Preprocessing may include:

* Encoding categorical variables
* Selecting relevant features
* Transforming highly skewed variables
* Scaling numerical features when required
* Preparing training and testing datasets

---

### 4. Unsupervised Learning

Unsupervised learning is used to explore hidden patterns within the forest fire dataset.

The goal is to determine whether observations with similar weather and fire-risk characteristics naturally form meaningful groups.

---

### 5. Supervised Machine Learning

Supervised machine learning is used to investigate whether forest fire outcomes can be predicted using environmental conditions and Fire Weather Index variables.

The models are trained and evaluated using appropriate performance metrics to determine how well the available features explain or predict forest fire outcomes.

---

### 6. Dashboard

An interactive dashboard will be created to communicate the most important findings from the analysis.

The dashboard will highlight:

* Forest fire activity
* Burned area
* Temperature and humidity
* Wind and rainfall
* Fire Weather Index indicators
* Important relationships discovered during EDA
* Key insights from the analysis

---

## Repository Structure

TEPP_Capstone_Project/
│
├── data/
│   ├── raw/
│   │   └── forestfires.csv
│   └── processed/
│       └── forest_fires_processed.csv
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_unsupervised_learning.ipynb
│   └── 04_supervised_model.ipynb
│
├── dashboard/
│   ├── Tableau dashboard file
│   └── dashboard screenshots
│
├── reports/
│   └── final_report.md  or PDF
│
├── README.md
└── requirements.txt





## Key Findings

🚧 **Analysis in Progress**

Final findings will be added after the exploratory analysis and machine learning stages are completed.

The analysis will focus on questions such as:

* Which environmental conditions are most strongly associated with larger burned areas?
* How do FFMC, DMC, DC, and ISI relate to forest fire behavior?
* How do temperature, humidity, wind, and rainfall relate to burned area?
* Are there identifiable groups of fires with similar environmental characteristics?
* Can machine learning models predict forest fire outcomes from the available features?

---

## Tools & Technologies

* **Python**
* **Pandas**
* **NumPy**
* **Matplotlib**
* **Seaborn**
* **Scikit-learn**
* **Jupyter Notebook**
* **Git & GitHub**
* **Power BI / Tableau**

---

## Project Status

🚧 **In Progress**

This project is being developed as part of the **The Knowledge House Phase 2 Data Analytics Capstone Project**.



