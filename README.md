# NVDA Time Series Forecasting: ARIMA, Prophet, and Monte Carlo Option Pricing

## Overview

This project applies a time series analysis and forecasting pipeline to **Nvidia (NVDA)** stock data using real historical prices from Yahoo Finance. It progresses from exploratory data analysis through statistical testing, ARIMA modelling, Facebook Prophet forecasting, Monte Carlo simulation of future price paths, and a Monte Carlo-based **European call option valuation** using market-sourced option parameters.

---

## What This Project Does

| Step | Description |
|------|-------------|
| 1 | Download and visualise 5-year NVDA daily price data |
| 2 | Narrow to post-2023 data to eliminate a structural break (the pre-2023 regime is far more stable and low relative to the AI-driven 2023+ run-up) |
| 3 | Test for seasonality (visual + ACF) |
| 4 | Test for stationarity (Augmented Dickey-Fuller) |
| 5 | Apply 1st-order differencing to achieve stationarity |
| 6 | Select ARIMA parameters (p, d, q) via ACF/PACF plots and AIC/BIC comparison |
| 7 | Fit and evaluate ARIMA(1,1,1) on an 80/20 train-test split |
| 8 | Build a Facebook Prophet model and compare its accuracy to ARIMA |
| 9 | Simulate future NVDA price paths via Monte Carlo |
| 10 | Price a European call option using Monte Carlo simulation |
| 11 | Re-price the option using parameters sourced from a real NVDA call (strike, implied volatility, expiry) |

---

## Methods Used

### Time Series Analysis
- **ACF / PACF plots** — Identify autocorrelation structure for ARIMA parameter selection
- **Augmented Dickey-Fuller (ADF) test** — Formally test for stationarity
- **1st-order differencing** — Transform the non-stationary series to stationary

### Forecasting Models
- **ARIMA (p, d, q)** — Classical parametric time series model
- **Facebook Prophet** — Additive decomposition model with trend and changepoint detection

### Derivative Pricing
- **Monte Carlo Simulation (GBM)** — Simulate future NVDA price paths and terminal values under a risk-neutral measure
- **European Call Option Pricing** — `max(S_T - K, 0)`, discounted at the risk-free rate

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
├── requirements.txt
├── LICENSE
├── .gitignore
└── README.md
```

---

## Installation

```bash
pip install -r requirements.txt
```

> Note: Prophet's backend (`cmdstanpy`) sometimes requires an extra build step depending on your OS — see the [Prophet installation guide](https://facebook.github.io/prophet/docs/installation.html) if `pip install` alone doesn't work.

---

## Key Results

- The raw NVDA price series is non-stationary (ADF p-value = 0.722); after 1st-order differencing it becomes stationary (ADF p-value ≈ 1.5×10⁻²⁷).
- Of the three ARIMA orders tested, **ARIMA(1,1,1)** has both the lowest AIC (3086.9) and lowest BIC (3100.1). ARIMA(2,1,2)'s additional AR/MA terms are statistically insignificant (p-values between 0.44 and 0.98).
- ARIMA(1,1,1) on the 80/20 train-test split gives an RMSE of **22.21** (improved from 24.29 after fixing a business-day frequency issue with `.asfreq()`).
- The Prophet model substantially outperforms ARIMA on this data, with an RMSE of **6.25**.
- The Monte Carlo European call valuation, run with parameters matching a real NVDA call (strike $110, 46.21% implied volatility, ~2-year expiry), produces a model price of **$44.73**.

---

## Limitations

- **No real market premium for comparison:** the final valuation uses real market-sourced *inputs* (strike, implied volatility, expiry) but the notebook does not compare the resulting $44.73 against an actual quoted market premium for that option — so this is a plausibility check on inputs, not a validation against a real price.
- **Volatility annualization:** an earlier Monte Carlo call (using volatility estimated directly from daily log returns) does not annualize that volatility before combining it with a multi-year time horizon, which likely understates the option's value in that specific run. The final comparison run avoids this by using an externally-sourced, already-annualized implied volatility instead.
- **Simulation count:** the price-path visualization uses 50 simulated paths and the option pricing uses 250 — enough to illustrate the method, but a larger count (e.g. 10,000+) would give a more stable, production-grade price estimate.
- **Log-return compounding:** the descriptive price-path simulation compounds log-return statistics using a simple-return formula (`(1 + r).cumprod()`). Log returns are technically additive in log-space (`exp(cumsum(r))`), so this section is illustrative rather than strictly precise — it doesn't affect the option pricing function, which uses the correct GBM form independently.

---

## Concepts Demonstrated

- **Time series stationarity testing and differencing**
- **ARIMA model identification and fitting**
- **Facebook Prophet** for trend-based forecasting
- **Geometric Brownian Motion** as a stochastic price process
- **Risk-neutral Monte Carlo pricing** for vanilla options
- **Model evaluation** using RMSE (scikit-learn)

---

## Author

**Mohammad Osama Khan**
MSc Financial Economics — Otto-von-Guericke University Magdeburg
[LinkedIn](https://www.linkedin.com/in/mohammad-osama-khan-93233a191) | mohammad2.khan@st.ovgu.de
