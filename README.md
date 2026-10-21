# 📈 Airline Passengers Time Series Analysis & Forecasting

An end-to-end time series analysis and forecasting project built with Python and Statsmodels. This project explores the classic Box-Jenkins methodology, statistical stationarity testing, variance/trend transformations, exponential smoothing, and comparative modeling using **ARIMA** and **SARIMA** on the Airline Passengers dataset (1949–1960).

---

## 📌 Project Overview

The objective of this project is to model and forecast monthly international airline passenger traffic. Through systematic statistical checks and exploratory decompositions, the project highlights why standard models fail on non-stationary, seasonal data and demonstrates how seasonal differencing and SARIMA yield accurate multi-step forecasts.

### Key Highlights
- **Exploratory Data Analysis (EDA):** Multiplicative decomposition into Trend, Seasonality, and Residual components.
- **Statistical Stationarity Verification:** Augmented Dickey-Fuller (ADF) hypothesis testing across transformations.
- **Data Transformations:** Log-transformations for variance stabilization and first/second-order differencing for trend removal ($p$-value $< 0.05$).
- **Model Identification:** Autocorrelation (ACF) and Partial Autocorrelation (PACF) evaluation for lag order determination.
- **Baseline Smoothing Techniques:** Simple Moving Average (SMA), Simple Exponential Smoothing (SES), and Holt-Winters Double Exponential Smoothing (DES).
- **Stochastic Modeling:** Non-seasonal ARIMA vs. Seasonal ARIMA (SARIMA).
- **Model Evaluation:** Root Mean Squared Error (RMSE) computation on un-transformed (exponential scale) passenger volumes.

---

## 📊 Dataset Description

The dataset records monthly totals of international airline passengers (in thousands) from **January 1949 to December 1960** (144 observations).

| Feature | Description | Type |
| :--- | :--- | :--- |
| `Month` | Date index formatted as `YYYY-MM` (Frequency: `MS`) | Datetime / Index |
| `Passengers` | Monthly passenger count in thousands | Integer / Float |

---

## ⚙️ Methodology & Pipeline

```
Raw Time Series 
   │
   ├──> Decomposition (Trend + Multiplicative Seasonality + Noise)
   │
   ├──> Stationarity Testing (ADF Test: p = 0.9918 -> Non-Stationary)
   │       │
   │       ├──> Log Transform (Variance Stabilisation)
   │       └──> 1st / 2nd Differencing (ADF Test: p < 0.05 -> Stationary)
   │
   ├──> Correlation Analysis (ACF / PACF Plots)
   │
   ├──> Benchmarking
   │       ├──> 12-Month Moving Average
   │       ├──> Simple & Double Exponential Smoothing (Holt-Winters)
   │       └──> ARIMA(1, 1, 1) & ARIMA(1, 2, 1)
   │
   └──> Final Production Model: SARIMA(1, 1, 1)(1, 1, 1, 12)
           │
           └──> Forecast Inversion (exp) & Metric Evaluation (RMSE: 48.27)
```

### 1. Stationarity Analysis (ADF Test)
- **Raw Data:** ADF Statistic: $0.815$, $p\text{-value}: 0.9918$ (Fail to reject $H_0$; highly non-stationary).
- **Log + 1st Difference:** ADF Statistic: $-2.717$, $p\text{-value}: 0.0711$.
- **Log + 2nd Difference:** ADF Statistic: $-8.196$, $p\text{-value}: 7.42 \times 10^{-13}$ (Reject $H_0$ at 1% significance level; strictly stationary).

### 2. Model Performance Summary
- **ARIMA (Non-Seasonal):** Captures long-term trajectory but fails to model recurring annual summer peaks and winter troughs.
- **Moving Average & Holt-Winters:** Effectively track the moving baseline but either introduce phase lag or underestimate seasonal amplitude.
- **SARIMA(1, 1, 1)(1, 1, 1, 12):** Captures both the macro trend and exact 12-month periodicity with high confidence intervals.
  - **Test Set Evaluation Metric:** **RMSE $\approx 48.27$**

---

## 🛠️ Tech Stack & Dependencies

- **Language:** Python 3.10+
- **Environment:** Jupyter Notebook / Google Colab
- **Core Libraries:**
  - `pandas` – Time series manipulation and date-time indexing
  - `numpy` – Mathematical transformations and array operations
  - `statsmodels` – ADF testing, seasonal decomposition, ARIMA, and SARIMAX architectures
  - `scikit-learn` – Error metrics (`mean_squared_error`)
  - `matplotlib` & `seaborn` – Plotting and diagnostic visualizations

---

## 🚀 Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/time-series-airline-passengers.git
   cd time-series-airline-passengers
   ```

2. **Create and activate a virtual environment (optional but recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate        # On Windows: venv\Scripts\activate
   ```

3. **Install required packages:**
   ```bash
   pip install numpy pandas matplotlib seaborn statsmodels scikit-learn
   ```

4. **Launch the notebook:**
   ```bash
   jupyter notebook "Project 6 Time Series Analysis & Forecasting.ipynb"
   ```


---

## 📈 Key Visualizations

The notebook produces key diagnostics to validate model assumptions:
1. **Multiplicative Decomposition Plot:** Deconstructs the underlying signal into level, yearly oscillation, and residual variance.
2. **ACF & PACF Correlograms:** Guides the selection of autoregressive $p$ and moving average $q$ parameters.
3. **Model Forecast vs Actual:** Compares out-of-sample predictions with 95% confidence bands against test data.

---
