# portfolio-optimization
Time Series Forecasting for Portfolio Management Optimization

GMF Investments — portfolio management project applying time series forecasting
(ARIMA/SARIMA + LSTM) and Modern Portfolio Theory to TSLA, BND, and SPY.

Project Structure

portfolio-optimization/
├── .vscode/
├── .github/workflows/unittests.yml
├── data/processed/
├── notebooks/          # exploratory + modeling notebooks (one per task)
├── src/                # reusable functions (data loading, metrics, models)
├── tests/              # unit tests for src/
├── scripts/            # CLI entry points
├── requirements.txt
└── README.md

Tasks

TaskDescriptionStatus1Data extraction, cleaning, EDA, stationarity testing, risk metrics Complete2ARIMA/SARIMA + LSTM forecasting models for TSLA 3Future forecast with confidence intervals + trend analysis 4Portfolio optimization (Efficient Frontier, MPT) 5Strategy backtesting vs. 60/40 SPY/BND benchmark

Setup

bashpython -m venv venv
source venv/bin/activate
pip install -r requirements.txt

Data

Historical daily OHLCV data for TSLA, BND, and SPY from 2015-01-01 to 2026-06-30,
pulled via the yfinance library. See notebooks/task1_eda.ipynb.


Raw data: data/raw/
Cleaned/processed data + figures: data/processed/


Documentation

Detailed write-ups for each completed task live in docs/:


docs/task1_report.md — data quality summary, EDA
insights, stationarity test results, and risk metrics for Task 1.


Notes


Data is split chronologically for modeling (train: 2015–2024, test: 2025–2026) —
no random shuffling, since order matters for time series.
TSLA is the sole forecasted asset (Task 2/3); BND and SPY use historical
average returns as their expected-return proxy in the portfolio optimization (Task 4).
## Tasks

| Task | Description | Status |
|------|-------------|--------|
| 1 | Data extraction, cleaning, EDA, stationarity testing, risk metrics | ✅ Complete |
| 2 | ARIMA/SARIMA + LSTM forecasting models for TSLA | ⬜ ARIMA done, LSTM pending |
| 3 | Future forecast with confidence intervals + trend analysis | ✅ Complete |
| 4 | Portfolio optimization (Efficient Frontier, MPT) | ✅ Complete |
| 5 | Strategy backtesting vs. 60/40 SPY/BND benchmark | ✅ Complete |

## Setup

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Data

Historical daily OHLCV data for TSLA, BND, and SPY from 2015-01-01 to 2026-06-30,
pulled via the `yfinance` library.

- Raw data: `data/raw/`
- Cleaned data, figures, and model outputs: `data/processed/`

## Documentation

Detailed write-ups for each task live in `docs/`:

- [`docs/task1_report.md`](docs/task1_report.md) — data quality summary, EDA
  insights, stationarity test results, and risk metrics.

## Key Findings Summary

- **Stationarity:** all three assets' price series are non-stationary (ADF
  p > 0.05); daily returns are stationary (p < 0.05) — confirming ARIMA
  requires differencing (d=1) on price series.
- **Risk profile:** TSLA had the highest annualized return (43.76%) and
  volatility (56.12%); BND was lowest-risk but slightly negative return
  (-0.82%); SPY sat in between (12.31% return, 17.39% volatility).
- **ARIMA forecast:** `auto_arima` selected ARIMA(0,1,0) for TSLA — equivalent
  to a random walk, with no exploitable linear pattern in price history alone.
  This produced a flat 12-month forecast with a widening confidence interval
  (±$132 at 1 month → ±$447 at 12 months).
- **Portfolio optimization:** given TSLA's near-zero forecasted return, the
  Max Sharpe portfolio allocated 100% to SPY (12.31% expected return, 17.39%
  volatility, Sharpe 0.71); Min Volatility allocated 94.3% BND / 5.7% SPY.
- **Backtest:** the Max Sharpe (100% SPY) strategy outperformed a 60/40
  SPY/BND benchmark over the most recent 12 months — 19.93% vs. 11.76% total
  return, Sharpe 1.56 vs. 1.42 — at the cost of a deeper max drawdown (-9.13%
  vs. -6.11%).

## Notes

- Data is split chronologically for modeling (train: 2015–2024, test:
  2025–2026) — no random shuffling, since order matters for time series.
- TSLA is the sole forecasted asset (Tasks 2/3); BND and SPY use historical
  average returns as their expected-return proxy in the portfolio
  optimization (Task 4).
- The LSTM component of Task 2 is being developed separately in Google Colab
  due to local bandwidth constraints around the TensorFlow dependency; results
  will be merged into the model comparison once complete.
- All backtest results reflect a single 12-month window with static
  (non-rebalanced) weights and no transaction costs — see `docs/` for full
  discussion of limitations.