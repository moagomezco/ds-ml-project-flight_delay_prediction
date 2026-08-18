# ✈️ Flight Delay Prediction — TunisAir

## Project Overview

Flight delays create operational challenges for airlines, airports, crews, and passengers.  
This machine learning project aims to **predict whether a flight is likely to be delayed and estimate the expected delay in minutes** using historical TunisAir flight records.

The goal is to support airline operations in identifying recurring bottlenecks, anticipating delays, and ultimately helping reduce the operational costs associated with frequent disruptions.

---

## Team

- Ariel 
- Hari
- Monica

---

## Business Problem

Flight delays can affect:

- aircraft utilization
- crew scheduling
- airport operations
- passenger connections
- customer satisfaction
- operational costs

The objective of this project is to use historical flight information to identify patterns associated with delays and build predictive models that can support more proactive operational decision-making.

### Main Question

> **Can historical flight and schedule information be used to predict flight delays and estimate their duration?**

---

## Dataset

The dataset comes from a **Zindi flight delay prediction challenge** and contains historical TunisAir flight records.

### Time Period

**2016–2018**

The target variable is:

- `target` — flight delay in minutes

The dataset includes information related to flight schedules, departure and arrival airports, aircraft, routes, dates, and other operational characteristics.

---


## Repository Structure

```text
.
├── 01_data_cleaning.ipynb
├── 02_EDA.ipynb
├── 03_models.ipynb
└── 04_test_prediction.ipynb
```

### Notebook Workflow

The notebooks should be run in order:

1. **`01_data_cleaning.ipynb`**  
   Clean and prepare the raw flight data before any analysis or modeling.

2. **`02_EDA.ipynb`**  
   Explore the cleaned data, investigate the target distribution, identify temporal patterns, study delays by airports and routes, and generate insights that guide feature engineering.

3. **`03_model_development.ipynb`**  
   Use the cleaned and engineered data to train, compare, and evaluate the machine learning models.

4. **`04_test.ipynb`**  
   Apply the selected final model to unseen/test data and evaluate or generate final predictions.

The project therefore follows the sequence:

```text
Raw Data
   ↓
Data Cleaning
   ↓
Exploratory Data Analysis
   ↓
Feature Engineering
   ↓
Model Training & Comparison
   ↓
Final Model
   ↓
Testing / Prediction
```

---

## Project Workflow

The project follows a standard machine learning workflow:

1. Data cleaning
2. Exploratory Data Analysis (EDA)
3. Temporal and operational feature engineering
4. Train/test preparation
5. Baseline regression modeling
6. Non-linear ensemble modeling
7. Model comparison
8. Feature importance analysis
9. Evaluation and stakeholder interpretation

---

## Exploratory Data Analysis

The EDA showed that flight delays are strongly right-skewed:

- most flights have relatively small delays
- a smaller number of flights experience much larger disruptions
- delay behavior varies over time
- recurring temporal patterns are present in the data

Because extreme delays can strongly influence regression metrics, the project also explored transformations and different modeling approaches to better understand the target distribution.

---

## Feature Engineering

The project uses calendar, flight, airport, route, and operational information to help the models detect patterns in delay behavior.

Examples of engineered or analyzed features include:

- departure and arrival airport
- month
- year
- hour
- weekday
- scheduled flight duration
- time of day
- rotation sequence
- weekend indicator
- route-related information
- temporal interval features

These features allow the models to capture both **temporal patterns** and **operational differences between flights**.

---

## Models Evaluated

Three main regression approaches were compared:

### Ridge Regression

A linear regularized regression model used as a baseline.

### Ridge Regression with Splines

A more flexible version of Ridge Regression designed to capture non-linear relationships.

### Histogram Gradient Boosting Regression Tree (HGBRT)

An ensemble tree-based model capable of learning complex and non-linear interactions between flight characteristics.

---

## Model Performance

| Model | R² | RMSE (min) | SD |
|---|---:|---:|---:|
| Ridge Regression | 0.04 | 80.06 | 13.01 |
| Ridge Regression + Splines | 0.00 | 76.93 | 78.27 |
| HGBRT | -0.12 | **71.50** | **3.82** |

### Interpretation

Among the tested models, **HGBRT achieved the lowest RMSE**, with an error of approximately **71.5 minutes**.

This suggests that the ensemble tree model is better able to capture complex patterns in the flight data than the linear regression approaches.

However, the negative R² indicates that the model still has limited ability to explain the full variation in flight-delay duration. This highlights the difficulty of predicting exact delay minutes using schedule and historical flight information alone.

---

## What the Predictions Show

The HGBRT predictions are able to follow part of the overall temporal delay pattern.

The model captures many periods with moderate delay levels, but it does not reproduce every large disruption accurately.

In particular:

- normal and moderate delays are easier to model
- sudden large delay spikes remain difficult to predict
- rare operational disruptions contribute significantly to prediction error

This is important when interpreting the model: it is more useful for identifying **general delay patterns and elevated risk** than for guaranteeing an exact delay duration for every flight.

---

## Feature Importance

The HGBRT feature-importance analysis indicates that the model is mainly influenced by a combination of **temporal, airport, and operational features**.

The most influential features are:

1. **Interval** — the strongest feature in the model, capturing temporal spacing or timing effects.
2. **Departure airport (`DEPSTN`)**
3. **Arrival airport (`ARRSTN`)**
4. **Month**
5. **Year**
6. **Hour of departure**
7. **Weekday**
8. **Flight duration**
9. **Time of day**
10. **Route / flight sequence information**, with a smaller contribution

This suggests that delays are strongly connected to **when a flight operates, where it departs from and arrives, and the operational characteristics of the scheduled flight**.

The importance of departure and arrival airports also suggests that airports may differ considerably in congestion, infrastructure, and operational capacity.

---

## Key Findings

- Flight delays show strong temporal variability.
- The target distribution is highly skewed, with a small number of very large delays.
- Airport and temporal characteristics are important predictors.
- Non-linear ensemble models outperform the tested linear baselines in terms of RMSE.
- HGBRT produced the lowest RMSE among the evaluated models.
- Large and unusual delays remain considerably harder to predict than regular operational delays.
- Historical schedule information provides useful signals, but it cannot explain every disruption.

---

## Stakeholder Takeaways

- **Machine learning can help identify flights with elevated delay risk.**
- **Airport and timing patterns provide useful operational signals.**
- The model can help airline teams identify recurring bottlenecks and periods associated with increased delay.
- Predictions should be treated as **decision support rather than exact guarantees**.
- The current models are more reliable for common delay behavior than for rare extreme disruptions.
- Better anticipation of recurring delays could support improved aircraft planning, crew planning, passenger communication, and resource allocation.
- Reducing recurring operational delays has the potential to reduce associated airline costs.

---

## Limitations

The main limitation of the current model is the prediction of **extreme, long, and rare delay events**.

Most flights belong to the normal or moderate-delay range, while very large delays occur much less frequently. As a result, the model has fewer examples from which to learn the patterns associated with these extreme events.

The model can capture many regular and recurring delay patterns, but unusual operational disruptions are much harder to anticipate accurately.

Several potentially important factors are also not included in the current dataset, such as:

- weather conditions
- airport size
- airport operational capacity
- technical aircraft issues
- crew availability
- air traffic control restrictions
- real-time airport disruptions

These missing factors may be especially important for explaining the largest and rarest delay events.

---

## Future Improvements

The next development steps should focus on adding information that better represents the operational environment around each flight.

### 1. Airport Size and Operational Capacity

Add information about airport characteristics, such as:

- airport size
- number of terminals or runways
- passenger or flight volume
- operational capacity
- congestion level

This could help explain why flights from some airports are more prone to delays than others.

### 2. Weather Information

Integrate historical weather data for departure and arrival airports.

Potential variables include:

- precipitation
- wind speed
- visibility
- storms
- temperature
- severe weather conditions

Weather information could be particularly valuable for understanding and predicting **extreme delay events** that cannot be explained by schedule information alone.

### 3. Time-Series Analysis

Implement a dedicated time-series analysis to better model the temporal dependence observed in the data.

This could help investigate:

- daily delay cycles
- weekly patterns
- seasonal effects
- delay accumulation during the day
- recurring periods of congestion
- changes in delay behavior over time

Time-series methods could complement the current machine learning models by explicitly modeling how delay behavior evolves over time.

### 4. Extreme-Delay Modeling

Because very long delays are rare, future work could explore a separate model specifically designed for extreme events.

This could include:

- classifying flights into normal and extreme-delay groups
- training a specialized model for extreme delays
- using alternative loss functions that give more weight to rare large errors

---

## Conclusion

This project demonstrates how historical flight data can be used to explore and predict airline delays.

Among the evaluated models, the **Histogram Gradient Boosting Regression Tree achieved the lowest RMSE of 71.50 minutes**, outperforming the Ridge Regression baselines on this metric.

The results also highlight an important practical limitation: **extreme delays are difficult to predict using historical schedule information alone**.

For airline stakeholders, the greatest value of the model is therefore not necessarily an exact minute-by-minute forecast, but the ability to identify recurring patterns, operational bottlenecks, and flights with potentially elevated delay risk.

---

## Main Data Source

Zindi — TunisAir Flight Delay Prediction Challenge

---
