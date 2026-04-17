# taxi-tip-prediction-big-data
# Predicting passenger tips using NYC HVFHS data

## Project Overview
This project builds an end-to-end big data ML pipeline to predict tipping behavior in NYC ride-hail trips. Using the NYC TLC High-Volume For-Hire Vehicle Service (HVFHS) dataset — **over 240 million trips across all 12 months of 2024** — the goal is to classify whether a passenger tip exceeds 10% of the base fare.

The pipeline spans cloud storage, distributed processing, feature engineering, and binary classification modeling, all built on Google Cloud Platform.

## Objective
The goal of this project is to predict whether a taxi tip is “good” (greater than 10% of the base fare) using trip and fare-related features. This is framed as a binary classification problem.

## Business Value
Understanding what drives higher tips can help:
- Drivers optimize pickup times and routes
- Platforms improve driver earnings strategies
- Improve customer satisfaction and service efficiency
- Support revenue optimization for ride services


## Approach
A logistic regression model is used to classify trips based on whether they result in a good tip. The analysis includes:
- Data cleaning and preprocessing
- Exploratory data analysis (EDA)
- Feature engineering
- Model training and evaluation

## Dataset
NYC TLC High-Volume For-Hire Vehicle Service (HVFHS) Trip Data  
Includes:
- Trip duration
- Fare amount
- Pickup and drop-off locations
- Service and dispatch details

## Pipeline Architecture

## Stage                 |Tool 
|---|---|
| Data Source            | NYC TLC HVFHS (Parquet, Jan–Dec 2024) |
| Cloud Storage          | Google Cloud Storage (GCS) |
| Distributed Processing | Google Cloud Dataproc |
| Modeling               | PySpark MLlib (Logistic Regression) |


## Cluster Setup

```bash
gcloud dataproc clusters create cluster-20e9 \
  --enable-component-gateway \
  --region us-central1 \
  --single-node \
  --master-machine-type n4-standard-8 \
  --master-boot-disk-type hyperdisk-balanced \
  --master-boot-disk-size 100 \
  --image-version 2.2-debian12 \
  --optional-components JUPYTER \
  --max-idle 3600s \
  --project kc-skypixel
```
## Features & Target

**Features:**
- `trip_miles`, `trip_duration`, `trip_weekend`, `pickup_hour`
- Fare-tier segments:
- `PULocationID`, `DOLocationID` (encoded across 262 TLC Taxi Zones)

**Target:** `tip_bucket` → `1` if tip > 10% of base fare, else `0`

**Train/test split:** 70/30


## Model Results
## | Metric    | Score |
|---|---|
   | Accuracy  | 90.5% |
   | Precision | 81.9% |
   | Recall    | 90.5% |
   | F1 Score  | 86.0% |
   | AUC       | 0.500 |
   | Cross-Val AUC | ~66.5% |
> **Note:** High accuracy with low AUC reflects class imbalance — a known limitation and area for future work.


## Key Findings

- **Time-of-day** (5–7 PM peak) and **fare-tier segmentation** were the strongest predictors of tipping
- **Pickup/drop-off location** significantly influenced tip likelihood
- Domain-driven feature engineering had more impact than model complexity
- Class imbalance should be addressed in future iterations (SMOTE, class weighting, or tree-based models)



## Business Impact

- **Drivers** can optimize earnings by targeting high-tip time windows and zones
- **Platforms** can refine dynamic pricing and incentive design
- **Service teams** gain a data-backed understanding of what drives passenger generosity


## How to Run This Project

1. Clone the repository
2. Install dependencies:
   pip install -r requirements.txt

3. Open Jupyter Notebook:
   jupyter notebook

4. Run notebooks in order:
   01 → 02 → 03 → 04

## Full Report
A detailed end-to-end report including data acquisition, EDA, feature engineering, and machine learning results is available below:
[View Full Report (PDF)](docs/NYC_Taxi_Tip_Prediction_GCP_Project_.pdf) 
