# CS207 Section 5 — Team 4 Project: Stock Movement Classification

A group project for CS207 exploring machine learning approaches to predicting stock price direction.

---

## Project Overview

This project frames next-day stock movement as a multi-class classification problem. Each team member is independently exploring different models and feature engineering approaches on the same underlying dataset (daily OHLCV bars for SPY, NVDA, MU, and TSLA from Yahoo Finance), with the goal of merging findings into a final combined analysis.

---

## Team Members & Notebooks

| Team Member | Notebook | Description |
|---|---|---|
| Pallavi | `Pallavi_Stock_Classification_Full_YahooDataSet.ipynb` | Full pipeline with 25 technical features, logistic regression baseline, and TensorFlow LSTM |
| Pallavi (prior) | `Pallavi_Stock_Classification_Full.ipynb` | Earlier version of Pallavi's notebook |
| Neil | `Neil_stock_ML.ipynb` | Neil's ML exploration |
| Ron | `Ron_Stock_ML.ipynb` | Data pipeline, EDA, and expanded features |
| Vansh | `stock_data_puller_vansh.ipynb` | Baseline and logistic regression work |

---

## Dataset

- **Source:** Yahoo Finance via `yfinance` (free, no API key required)
- **Symbols:** SPY, NVDA, MU, TSLA
- **Date range:** 2016 – 2026 (~10 years of daily bars)
- **Target:** Next-day return quintile (5-class classification)

---

## How to Run

Each notebook is self-contained and runs end-to-end in Google Colab:

1. Open the notebook of interest using the **Open in Colab** badge (or upload to Colab manually).
2. Run **Runtime > Run all**.
3. No API keys are needed — data is pulled automatically from Yahoo Finance.

---

## Project Status

Work in progress. Team members are actively developing their individual notebooks. A final merged analysis will be added here.

---

*CS207 · Section 5 · Team 4*
