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