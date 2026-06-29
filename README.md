# Time Series Forecasting: ARIMA, SARIMA & ETS Model Comparison

A rigorous time series analysis and forecasting project comparing three 
model families on the classic AirPassengers benchmark dataset.  
Built as part of the Master in Business Analytics program at 
Mediterranean School of Business (MSB), Tunis — March 2025.

---

## Project Overview

This project forecasts monthly international airline passenger numbers 
(1949–1960) using three time series models: ARIMA, SARIMA, and ETS.  
The goal is to demonstrate the full forecasting pipeline — from 
exploration and preprocessing through model selection, residual 
diagnostics, and test set evaluation — and to compare model performance 
using standard accuracy metrics.

This is a methods demonstration project using a well-known benchmark 
dataset, not a live business engagement.

---

## Dataset

- **Source:** Built-in R dataset (`AirPassengers`)
- **Description:** Monthly totals of international airline passengers
- **Period:** January 1949 – December 1960
- **Observations:** 144 monthly data points
- **Split:**
  - Training: Jan 1949 – Dec 1957
  - Validation: Jan 1958 – Dec 1958
  - Test: Jan 1959 – Dec 1960

---

## Methodology

### 1. Data Exploration
- Time series plot: clear upward trend + strong 12-month seasonality
- STL decomposition: separated trend, seasonal, and remainder components
- ACF / PACF plots: identified autocorrelation structure

### 2. Preprocessing
- **Log transformation** — stabilized variance across the series
- **First-order differencing** — removed trend to achieve stationarity
- **Stationarity tests:**
  - ADF test → stationary (p < 0.05)
  - KPSS test → stationary confirmed

### 3. Models Implemented

| Model | Specification |
|---|---|
| ARIMA | ARIMA(2,1,1) — best candidate from grid search |
| SARIMA | SARIMA(1,0,0)(0,1,1)[12] |
| ETS | ETS(A,A,A) — additive error, trend, and seasonality |

### 4. Model Selection Process
- Fitted 5 candidate ARIMA models on training set
- Evaluated each on validation set using RMSE, MAPE, AIC, BIC
- Selected best specification per model family before final test evaluation

### 5. Residual Diagnostics (ETS & SARIMA)
- **Ljung-Box test:** p = 0.3994 → residuals are white noise (no autocorrelation)
- **Shapiro-Wilk test:** p = 0.1669 → residuals approximately normally distributed
- **ACF/PACF of residuals:** no significant spikes
- **Homoscedasticity:** constant variance confirmed visually

---

## Results

### Validation Set Performance

| Model | RMSE | MAPE | AIC | BIC |
|---|---|---|---|---|
| ARIMA(2,1,1) | 0.16 | 0.02% | -188.18 | -177.49 |
| SARIMA(1,0,0)(0,1,1)[12] | 0.10 | 0.02% | **-352.32** | **-339.50** |
| ETS(A,A,A) | **0.08** | **0.01%** | -194.80 | -149.20 |

### Test Set Performance

| Model | RMSE | MAPE |
|---|---|---|
| ETS(A,A,A) | 0.07 | 0.01% |
| SARIMA(1,0,0)(0,1,1)[12] | **0.03** | **0.00%** |

### Key Finding
- **ETS(A,A,A)** achieved the best validation accuracy (RMSE = 0.08)
- **SARIMA** outperformed on the test set (RMSE = 0.03), demonstrating 
  superior generalization and seasonal pattern capture
- **ARIMA** (non-seasonal) underperformed on both sets — confirms that 
  ignoring the 12-month seasonal structure is a significant limitation 
  for this dataset

---

## Recommendation

**Use SARIMA** when maximum forecast accuracy is the priority — it 
handles both trend and 12-month seasonality explicitly and generalizes 
better to unseen data.

**Use ETS** when simplicity and interpretability matter — it is easier 
to explain to non-technical stakeholders and performs nearly as well.
