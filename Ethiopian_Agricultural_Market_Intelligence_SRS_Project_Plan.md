# 🇪🇹 Ethiopian Agricultural Market Intelligence & Food Price Risk Prediction

## Software Requirements Specification (SRS) + Project Execution Plan

---

## 1. Project Overview

### 1.1 Project Title

**Ethiopian Agricultural Market Intelligence & Food Price Risk Prediction**

### 1.2 Main Goal

Build a data-driven system that:

- Analyzes historical food prices across Ethiopian markets.
- Discovers price trends and patterns.
- Predicts future food prices.
- Identifies markets/products with higher price risk.
- Provides market intelligence through a dashboard.

### 1.3 Main Target

The main target dataset is:

`02_staple_food_price.csv`

The target variable is:

`value` — food price.

Supporting datasets will later be evaluated:

- `01_ethiopian_rainfall.csv`
- `03_population_region_sex_2007_2022.xlsx`
- `05_producer_price.csv`
- `06_trade.csv`

---

# 2. System Objectives

The system should answer questions such as:

### Price Analysis
- What is the current/historical price?
- How has the price changed over time?
- Which products have increasing or decreasing trends?

### Forecasting
- What might the future price be?
- How accurately can future prices be predicted?

### Risk Analysis
- Which market/product combinations show high price volatility?
- Which products may experience significant future price increases?

### Market Intelligence
- Which markets have relatively high prices?
- Which markets have relatively low prices?
- Where are significant and persistent price differences observed?

---

# 3. Datasets

## 3.1 Staple Food Price — Main Dataset

File:

`02_staple_food_price.csv`

Important fields:

- `market`
- `admin_1`
- `longitude`
- `latitude`
- `product`
- `period_date`
- `price_type`
- `product_source`
- `unit`
- `currency`
- `value`

Target:

`value`

---

## 3.2 Rainfall

File:

`01_ethiopian_rainfall.csv`

Potential features include rainfall-related measurements such as:

- `rfh`
- `rfh_avg`
- `r1h`
- `r1h_avg`
- `r3h`
- `r3h_avg`
- `rfq`

Purpose:

**Climate information**

---

## 3.3 Population

File:

`03_population_region_sex_2007_2022.xlsx`

Purpose:

**Demand/population context**

---

## 3.4 Producer Price

File:

`05_producer_price.csv`

Purpose:

**Agricultural producer/economic price context**

---

## 3.5 Trade

File:

`06_trade.csv`

Purpose:

Potential macroeconomic/trade context.

> Trade should not automatically be included. It should be tested to determine whether it actually improves prediction.

---

# 4. Exact Project Execution Sequence

Follow this order:

```text
PHASE 1
Project Setup
      ↓
PHASE 2
Data Understanding
      ↓
PHASE 3
Food Price Data Cleaning
      ↓
PHASE 4
Exploratory Data Analysis
      ↓
PHASE 5
Time-Series Preparation
      ↓
PHASE 6
Feature Engineering
      ↓
PHASE 7
Supporting Dataset Cleaning
      ↓
PHASE 8
Dataset Integration
      ↓
PHASE 9
Train / Validation / Test Split
      ↓
PHASE 10
Scaling + Sequence Creation
      ↓
PHASE 11
Baseline Models
      ↓
PHASE 12
LSTM Model
      ↓
PHASE 13
Model Evaluation
      ↓
PHASE 14
Risk & Market Intelligence
      ↓
PHASE 15
Dashboard
      ↓
PHASE 16
Documentation + Deployment
```

---

# PHASE 1 — PROJECT SETUP

## Step 1. Create project structure

```text
ethiopian-agricultural-market-intelligence/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│
├── src/
│
├── models/
│
├── dashboard/
│
├── reports/
│
├── requirements.txt
│
└── README.md
```

## Step 2. Set up tools

### Data storage
Google Drive

### Analysis
Google Colab

### Version control
GitHub

Raw datasets should generally remain outside the GitHub repository.

---

# PHASE 2 — DATA UNDERSTANDING

For each dataset, eventually perform:

1. Load data.
2. Check shape.
3. Check columns.
4. Check data types.
5. Check missing values.
6. Check duplicates.
7. Check unique values.
8. Check numerical statistics.
9. Check date range.
10. Understand geographic levels.
11. Understand units.
12. Identify target and features.

---

# PHASE 3 — FOOD PRICE DATA CLEANING

This is the current main phase.

## Completed

- Loading and inspection — **DONE**
- Missing-value analysis — **DONE**
- Market-product grouping — **DONE**
- 50% missing-data filtering — **DONE**
- Date conversion — **DONE**
- Consecutive-gap analysis — **DONE**

Current filtered dataset:

- **56,896 rows**
- **3,658 missing prices**

## Step 3.1 — Remove useless columns

Example:

`Unnamed: 15`

This column was completely empty and should not be useful for modeling.

## Step 3.2 — Fix data types

Important:

- `period_date` → datetime
- `value` → numeric

## Step 3.3 — Handle missing values

First understand missingness.

Separate:

- Short gaps
- Medium gaps
- Long gaps

### Short gaps

Potentially use time interpolation.

### Long gaps

Do not blindly interpolate.

Examples already identified include:

- Gambella + Sorghum (Red) → longest gap = 84
- Dire Dawa, Kezira + several products → longest gap = 53
- Warder + Wheat Grain → longest gap = 43
- Shashemene + Sorghum (White) → longest gap = 43
- Dessie + Sorghum (Red) → longest gap = 42

## Step 3.4 — Check outliers

Possible methods:

- IQR
- Z-score
- Time-series visualization

Do not automatically delete every outlier because a large price change may represent a real economic event.

## Step 3.5 — Check consistency

Check:

- currency
- unit
- unit type
- price type
- product source
- product names
- market names

## Step 3.6 — Re-check duplicates

Confirm there are no duplicate observations after cleaning.

---

# PHASE 4 — EXPLORATORY DATA ANALYSIS

Perform EDA only after the main cleaning is completed.

## 4.1 Overall price distribution

Calculate and visualize:

- mean
- median
- minimum
- maximum
- distribution

## 4.2 Price by product

Compare products such as:

- Teff
- Wheat
- Maize
- Sorghum
- Beans

and others present in the dataset.

## 4.3 Price by market

Identify markets with relatively high or low historical prices.

## 4.4 Price trends over time

Plot:

`Date → Price`

## 4.5 Product-market trends

Analyze individual market-product combinations.

Example:

`Addis Ababa, Merkato + Wheat`

## 4.6 Volatility

Calculate measures such as:

- standard deviation
- coefficient of variation
- rolling volatility

## 4.7 Seasonality

Investigate:

- month
- season
- year
- recurring price patterns

---

# PHASE 5 — TIME-SERIES PREPARATION

This phase is essential for LSTM.

## 5.1 Define the forecasting unit

Use:

`market + product`

as a separate time series where appropriate.

## 5.2 Sort chronologically

Sort by:

1. market
2. product
3. date

## 5.3 Check frequency

Determine whether observations are consistently weekly.

## 5.4 Distinguish missing prices from missing dates

These are different problems.

### Missing price

A date exists but `value` is missing.

### Missing date

An expected observation itself is absent.

## 5.5 Establish the appropriate time index

Where appropriate, create a consistent weekly timeline before sequence generation.

---

# PHASE 6 — FEATURE ENGINEERING

Create useful predictive features.

## 6.1 Price lag features

Possible features:

- `price_lag_1`
- `price_lag_2`
- `price_lag_4`
- `price_lag_8`
- `price_lag_12`

These represent previous weeks' prices.

## 6.2 Rolling statistics

Possible features:

- `rolling_mean_4`
- `rolling_mean_8`
- `rolling_std_4`
- `rolling_std_8`

## 6.3 Trend features

Possible features:

- price change
- price growth
- price trend

## 6.4 Calendar features

Possible features:

- year
- month
- week
- quarter
- season

Potentially use cyclical encoding:

- `sin(month)`
- `cos(month)`

---

# PHASE 7 — SUPPORTING DATASET CLEANING

After the main food-price dataset is understood and cleaned, process the other datasets separately.

Datasets:

- Rainfall
- Population
- Producer Price
- Trade

For each:

```text
Load
 ↓
Understand
 ↓
Clean
 ↓
Validate
 ↓
Save processed version
```

Do not merge them immediately.

---

# PHASE 8 — DATASET INTEGRATION

This may be one of the most difficult phases because the datasets have different time and geographic levels.

## 8.1 Geographic alignment

Determine relationships among:

- market
- ADM2
- region
- national level

## 8.2 Temporal alignment

Food price is approximately weekly, while some supporting datasets are yearly or more frequent.

For example, annual population information can be associated with weekly observations within the same year, but the assumption must be documented.

## 8.3 Rainfall aggregation

Because rainfall is more frequent than food prices, aggregate it appropriately to the food-price time scale.

## 8.4 Producer price alignment

Align annual producer price information with the corresponding food-price period while recognizing that this is a coarse feature.

## 8.5 Test feature usefulness

Do not assume all datasets improve the model.

Progressively test:

```text
Model A:
Food price only

Model B:
Food price + rainfall

Model C:
Food price + rainfall + population

Model D:
Food price + rainfall + population + producer price

Model E:
Add trade only if justified
```

Compare their actual performance.

---

# PHASE 9 — TRAIN / VALIDATION / TEST SPLIT

Never randomly split time-series data.

Use chronological splitting.

Example:

```text
Train          Validation          Test
 70%              15%               15%
```

Conceptually:

```text
2020 ───────── 2021 ─────── 2022 ─────── 2023
|---------------|------------|-------------|
     TRAIN        VALIDATION       TEST
```

The exact dates must be determined from the final usable data.

---

# PHASE 10 — SCALING + LSTM SEQUENCE CREATION

## 10.1 Scaling

Possible methods:

- MinMaxScaler
- StandardScaler

Important:

**Fit the scaler only on training data.**

This prevents data leakage.

## 10.2 Sequence creation

Choose a lookback window.

Example:

`lookback = 12`

Meaning:

```text
Past 12 weeks → predict the next week
```

Concept:

```text
Week 1
Week 2
...
Week 12
   ↓
 LSTM
   ↓
Week 13 prediction
```

The final lookback should be tested rather than assumed.

---

# PHASE 11 — BASELINE MODELS

Before LSTM, establish simple baselines.

## Baseline 1 — Naive

Next price = last observed price.

## Baseline 2 — Moving Average

Prediction = recent average.

Potential machine-learning baselines can also be tested, such as:

- Linear Regression
- Random Forest
- XGBoost

The exact baseline set should match the final feature structure.

### Why baselines matter

If LSTM does not improve on a simple baseline, investigate why before claiming the deep-learning model is useful.

---

# PHASE 12 — LSTM MODEL

The main deep-learning model.

Possible starting architecture:

```text
Input Sequence
      ↓
LSTM 128
      ↓
Dropout
      ↓
LSTM 64
      ↓
Dense
      ↓
Predicted Price
```

This is a starting architecture, not a guaranteed optimal architecture.

Experiment with:

- lookback
- number of LSTM layers
- hidden units
- dropout
- batch size
- learning rate
- epochs

Use validation data to guide model selection.

---

# PHASE 13 — MODEL EVALUATION

Use multiple metrics.

## MAE

Mean Absolute Error.

Useful because it is expressed in the same general price units as the target.

## RMSE

Root Mean Squared Error.

Penalizes larger errors more strongly.

## R²

Measures explained variance, but should not be the only metric for time-series forecasting.

Compare:

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| Naive | — | — | — |
| Moving Average | — | — | — |
| ML Model | — | — | — |
| LSTM | — | — | — |

Fill this table only with actual experiment results.

---

# PHASE 14 — PRICE RISK ANALYSIS

Prediction alone is not the complete goal.

Develop a quantitative risk framework based on measurable information such as:

- predicted price change
- historical volatility
- forecast uncertainty/error

Possible categories:

- Low risk
- Moderate risk
- High risk

The thresholds must be defined quantitatively and documented.

---

# PHASE 15 — MARKET INTELLIGENCE

Analyze:

## Market price differences

Compare prices across markets.

## Price trends

Classify measurable trends such as:

- increasing
- stable
- decreasing

## Market opportunities

Identify significant and persistent price differences.

However, price differences should not automatically be interpreted as profitable trading opportunities because they can result from:

- transportation costs
- product quality
- market conditions
- timing
- data problems
- other economic factors

---

# PHASE 16 — DASHBOARD

Possible dashboard:

## Overview

**Ethiopian Agricultural Market Intelligence**

## Current Price

Show:

- product
- market
- current price

## Forecast

Display:

- historical price
- predicted price

Example:

```text
Actual Price ────────────
                       ╲
                        ╲ Forecast
                         ┄┄┄┄┄┄┄┄
```

## Risk

Show:

- product
- market
- risk level
- expected price change

## Market Comparison

Compare selected markets.

## Climate Context

Show rainfall information if it proves useful to the final predictive model.

---

# PHASE 17 — DEPLOYMENT

Possible technology stack:

### Data Science

- Python
- Pandas
- NumPy
- Scikit-learn
- TensorFlow/Keras

### Dashboard

- Streamlit

### Optional API/backend

- Flask
- FastAPI

Deployment comes after the model and dashboard are validated.

---

# PHASE 18 — DOCUMENTATION

Final report structure:

1. Introduction
2. Problem Statement
3. Objectives
4. Data Sources
5. Data Understanding
6. Data Cleaning
7. Exploratory Data Analysis
8. Feature Engineering
9. Data Integration
10. Methodology
11. Baseline Models
12. LSTM Model
13. Model Evaluation
14. Risk Analysis
15. Market Intelligence
16. Dashboard
17. Results
18. Limitations
19. Future Work
20. Conclusion

---

# CURRENT PROJECT STATUS

```text
Project Setup
        ✅
        ↓
Data Understanding
        ✅
        ↓
Food Price Data Cleaning
        ↓
Missing-value analysis
        ✅
50% filtering
        ✅
Date conversion
        ✅
Consecutive-gap analysis
        ✅
        ↓
Interpolation
        ⏳ CURRENT
        ↓
Outlier analysis
        ⏳
        ↓
Consistency checks
        ⏳
        ↓
EDA
        ⏳
        ↓
Time-series preparation
        ⏳
        ↓
Feature engineering
        ⏳
        ↓
Supporting datasets
        ⏳
        ↓
Integration
        ⏳
        ↓
Train / Validation / Test
        ⏳
        ↓
Baseline models
        ⏳
        ↓
LSTM
        ⏳
        ↓
Evaluation
        ⏳
        ↓
Risk analysis
        ⏳
        ↓
Dashboard
        ⏳
        ↓
Deployment & Documentation
        ⏳
```

# Important Working Rules

1. Work on **one dataset at a time**.
2. Currently focus only on `02_staple_food_price.csv`.
3. Do not jump to LSTM before data preparation is complete.
4. Explain **what**, **why**, and **how** for every important step.
5. Do not blindly delete data.
6. Do not blindly fill missing values with mean or median.
7. Do not blindly interpolate long missing periods.
8. Avoid time-series data leakage.
9. Never randomly split the time series.
10. Test whether additional datasets actually improve prediction.
11. After each code step, inspect the output before continuing.
12. Keep the workflow reproducible and document important decisions.

# Immediate Next Step

The current task is:

**Learn and implement interpolation for appropriate short missing gaps in `02_staple_food_price.csv`.**

Sequence:

```text
Understand interpolation
        ↓
Identify appropriate short gaps
        ↓
Interpolate
        ↓
Verify results
        ↓
Check remaining missing values
        ↓
Continue to outlier analysis
```
