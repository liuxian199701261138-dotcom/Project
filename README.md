# Crash Risk Prediction Using Spatial Analysis and Machine Learning

## Overview

This project develops a spatially informed machine learning pipeline to identify and predict high-risk traffic crash districts at the census tract level. By integrating spatial statistics (Moran’s I, Getis-Ord Gi*) with machine learning (Random Forest classification), the system captures both geographic clustering effects and socio-demographic exposure factors to support data-driven traffic safety planning.

The project is motivated by urban traffic safety initiatives (e.g., Vision Zero) and demonstrates how AI can be used to proactively identify areas at elevated crash risk, rather than relying solely on historical hotspot mapping.

---

## Key Features

* Census-tract-level crash rate computation
* Spatial autocorrelation analysis (Global Moran’s I)
* Hotspot detection (Getis-Ord Gi*)
* Spatial feature engineering (log transforms and spatial lags)
* Supervised ML risk classification (Random Forest)
* Risk probability mapping for decision support

---

## Data Sources

* **Traffic crash data**: Aggregated crash counts by census tract
* **Census tract geometries**: TIGER/Line shapefiles
* **Population data**: Used to compute per-capita crash rates

> ⚠️ Raw data files are not included in this repository due to size/licensing constraints.

---

## Methodology

### 1. Data Preprocessing

* Spatial join of crash points to census tracts
* Computation of:

  * `crashes_per_1000`
  * `log_crash_rate`
  * `population_density`
  * `log_pop_density`
* Removal of tracts with missing or zero-exposure values

### 2. Spatial Analysis

* Construction of Queen contiguity spatial weights
* Global Moran’s I to assess spatial clustering
* Getis-Ord Gi* to identify statistically significant crash hotspots

### 3. Feature Engineering

* Log transformations to stabilize skewed distributions
* Spatial lag features:

  * `lag_log_crash_rate`
  * `lag_log_pop_density`

### 4. Machine Learning Model

* **Model**: Random Forest Classifier
* **Target**: `high_risk` (binary indicator derived from Gi* significance)
* **Evaluation Metric**: ROC-AUC
* **Output**:

  * Risk probability per census tract
  * Feature importance analysis

---

## Results Summary

* **Global Moran’s I**: ~0.20 (p-value = 0.001), indicating statistically significant spatial clustering
* **Model Performance**:

  * ROC-AUC: 1.00
* **Top Predictive Features**:

  1. `log_crash_rate`
  2. `lag_log_crash_rate`
  3. `population`
  4. `log_pop_density`

These results demonstrate that combining spatial context with ML significantly improves crash risk identification.

---

## Repository Structure

```
crash-risk-project/
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_spatial_analysis.ipynb
│   ├── 03_feature_engineering.ipynb
│   ├── 04_ml_model.ipynb
│   └── 05_visualization.ipynb
├── requirements.txt
├── README.md
└── demo/
```

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/crash-risk-project.git
cd crash-risk-project
```

### 2. Create a virtual environment (recommended)

```bash
conda create -n crash-risk python=3.11
conda activate crash-risk
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

## Usage

1. Run notebooks sequentially in the `notebooks/` directory
2. Ensure spatial weights are constructed before running ML notebooks
3. Final outputs include:

   * Risk probability maps
   * Feature importance tables

---

## AI Tools Usage Disclosure

This project leveraged AI-assisted development tools for:

* Debugging Python and GeoPandas errors
* Refining spatial feature engineering strategies
* Improving ML evaluation methodology

All modeling decisions, validation, and interpretations were conducted and verified by the author.

---

## Limitations

* Potential overfitting due to strong spatial dependence
* Binary risk labels derived from statistical thresholds
* Limited behavioral and temporal traffic variables

Future work may include temporal modeling, deep learning on road networks, and fairness-aware risk prediction.

---

## Societal Impact

This system supports:

* Proactive traffic safety interventions
* Equitable allocation of enforcement and infrastructure resources
* Data-driven urban planning decisions

By identifying risk patterns before severe outcomes occur, this work contributes to safer and more resilient cities.

---

## License

This project is released for academic use only.
