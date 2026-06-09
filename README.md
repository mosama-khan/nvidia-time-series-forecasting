# NVDA Time Series Forecasting: ARIMA, Prophet, and Monte Carlo Option Pricing

## Overview

This project applies a full time series analysis and forecasting pipeline to **Nvidia (NVDA)** stock data using real historical prices from Yahoo Finance. It progresses from exploratory data analysis through statistical testing, ARIMA modelling, Facebook Prophet forecasting, Monte Carlo simulation for future price paths, and finally **European call option valuation** using Monte Carlo methods — compared against real-world option pricing.

---

## What This Project Does

| Step | Description |
|------|-------------|
| 1 | Download and visualise 5-year NVDA daily price data |
| 2 | Narrow to post-2023 data to eliminate structural breaks |
| 3 | Test for seasonality (visual + ACF) |
| 4 | Test for stationarity (Augmented Dickey-Fuller) |
| 5 | Apply 1st-order differencing to achieve stationarity |
| 6 | Select ARIMA parameters (p, d, q) via ACF/PACF plots |
| 7 | Fit and evaluate ARIMA model |
| 8 | Build Facebook Prophet model with trend/changepoint analysis |
| 9 | Simulate future NVDA price paths via Monte Carlo (GBM) |
| 10 | Price a European call option using Monte Carlo simulation |
| 11 | Compare simulated option price against real market price |

---

## Methods Used

### Time Series Analysis
- **ACF / PACF plots** — Identify autocorrelation structure for ARIMA parameter selection
- **Augmented Dickey-Fuller (ADF) test** — Formally test for stationarity
- **1st-order differencing** — Transform non-stationary series to stationary

### Forecasting Models
- **ARIMA (p, d, q)** — Classical parametric time series model
- **Facebook Prophet** — Additive decomposition model with trend + seasonality + holidays

### Derivative Pricing
- **Monte Carlo Simulation (GBM)** — Simulate thousands of future NVDA price paths under risk-neutral measure
- **European Call Option Pricing** — `max(S_T - K, 0)` discounted at risk-free rate
- **Real-world option comparison** — Validate Monte Carlo output against live market prices

---

## Tech Stack

```
Python 3 | yfinance | pandas | numpy | statsmodels | prophet | matplotlib | scikit-learn
```

---

## Project Structure

```
nvidia-time-series-forecasting/
│
├── nvidia_time_series_forecasting.ipynb   # Full analysis and modelling notebook
└── README.md
```

---

## Installation

```bash
pip install yfinance matplotlib pandas statsmodels numpy prophet scikit-learn
```

---

## Key Findings

- NVDA pre-2023 data was excluded due to a structural break — the AI-driven surge from 2023 onward represents a fundamentally different price regime.
- No seasonality detected via visual inspection and ACF.
- Series was non-stationary in levels but stationary after 1st-order differencing (d=1).
- Monte Carlo option prices converge to market prices as simulation count increases.

---

## Concepts Demonstrated

- **Time series decomposition** (trend, seasonality, residuals)
- **ARIMA model identification and fitting**
- **Facebook Prophet** for non-linear trend modelling
- **Geometric Brownian Motion** as a stochastic price process
- **Risk-neutral Monte Carlo pricing** for vanilla options
- **Model evaluation** using RMSE (scikit-learn)

---

## Author

**Mohammad Osama Khan**
MSc Financial Economics — Otto-von-Guericke University Magdeburg
[LinkedIn](https://www.linkedin.com/in/mohammad-osama-khan-93233a191) | mohammad2.khan@st.ovgu.de
