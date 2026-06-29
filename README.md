# HarshavardaK_TimeSeriesStudyofAQI-AIrPurifierSalesInteractions_RTSMProject_April26

# 📈 Modeling Delhi AQI Trends and Air Purifier Revenue Forecasting

A time series forecasting project that models **Delhi's Air Quality Index (AQI)** using historical PM2.5 data and analyzes its relationship with **Blue Star air purifier sales revenue**. The project combines classical statistical forecasting techniques with business analytics to demonstrate how environmental indicators can drive consumer demand and strategic decision-making.

---

## Project Overview

Poor air quality has become a recurring issue in Delhi, especially during winter months. As pollution levels increase, consumer demand for air purifiers also rises.

This project investigates this relationship by:

* Forecasting daily AQI values using historical pollution data
* Forecasting daily revenue from air purifier sales
* Quantifying the relationship between AQI and consumer purchasing behavior
* Demonstrating how environmental data can support inventory planning and marketing decisions

---

## Objectives

* Build accurate forecasting models for Delhi AQI
* Forecast air purifier revenue using time series methods
* Compare direct ARMA forecasting with STL decomposition based models
* Analyze seasonal consumer behavior driven by pollution
* Generate actionable business insights from environmental data

---

## Dataset

### AQI Dataset

* Daily PM2.5 observations
* Converted into AQI using EPA breakpoint methodology
* Time period: **2018 – 2023**

### Revenue Dataset

* Daily Blue Star air purifier revenue
* Time period: **2018 – 2023**

---

## Methodology

### Data Preprocessing

* Missing value treatment
* AQI computation from PM2.5
* Daily resampling
* Forward filling and interpolation
* Rolling median smoothing (Revenue)

### Stationarity Testing

The Augmented Dickey-Fuller (ADF) test was used to verify stationarity before model fitting.

### Forecasting Models

#### AQI Forecasting

* ARMA Model
* STL Decomposition
* ARMA on Residuals
* Forecast reconstruction

#### Revenue Forecasting

* STL Decomposition
* Residual ARMA Modeling
* Confidence interval estimation

---

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Statsmodels
* Scikit-learn

---

## Key Results

### AQI Forecasting

* Best direct model: **ARMA(3,5)**
* STL + ARMA reduced forecasting RMSE by approximately **41%**

### Revenue Forecasting

* Best residual model: **ARMA(4,5)**
* Successfully captured seasonal demand fluctuations
* Produced reliable confidence intervals for business forecasting

---

## Business Insights

* Strong positive correlation between AQI and air purifier revenue
* Highest demand occurs during winter pollution spikes
* AQI can serve as a leading indicator for inventory planning
* STL decomposition significantly improves forecasting performance
* Environmental analytics can enhance supply chain and marketing decisions

---

## Repository Structure

```
├── data/
│   ├── delhi_aqi.csv
│   └── revenue.csv
│
├── notebooks/
│   ├── AQI_Model.ipynb
│   └── Revenue_Model.ipynb
│
├── figures/
│
├── report/
│   └── RTSM_Report.pdf
│
├── requirements.txt
└── README.md
```

---

## Future Improvements

* Compare ARMA with SARIMA and Prophet
* Incorporate weather variables (temperature, humidity, wind speed)
* Apply LSTM and Transformer-based forecasting models
* Develop a real-time forecasting dashboard
* Extend the framework to multiple cities

---

## Authors

* Harshavarda Kumarasamy
* Ritvik Puvvada
* Vasa Harish
* Vangala Akshay Reddy

---

## Report

The complete project report, including methodology, derivations, statistical analysis, and forecasting results, is included in this repository.

