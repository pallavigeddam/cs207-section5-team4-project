# CS207 Section 5 — Team 4 Project: Stock Movement Classification

A group project for CS207 exploring machine learning approaches to predicting stock price direction.

---

## Project Overview

This project asks whether the direction of the next price move can be predicted from historical market data, and whether the answer changes with the time horizon. Each team member independently explores different models and feature-engineering approaches across a range of horizons, from sub-second limit order book depth up through minute, hourly, daily, weekly, and monthly bars, using data from Yahoo Finance, Alpaca, and Databento. Throughout we separate direction (which way price moves) from magnitude (how large the move is), with the individual findings merged into a final combined analysis.

---

## Team Members & Notebooks

| Team Member | Notebook | Description |
|---|---|---|
| Pallavi | `Pallavi_Stock_Classification.ipynb` | Shared data pipeline and frozen datasets; four-horizon protocol; logistic regression, random forest, XGBoost, LSTM, ensemble, and meta-labeling; subgroup, era, and walk-forward analysis |
| Neil | `Neil_Stock_ML_Final.ipynb` | Minute-scale random forest study; feature-count comparison; confidence-threshold sweeps |
| Ron | `Ron_LSTM.ipynb` | DeepLOB with order-flow branch; order-book pipeline; leakage audit and label-artifact diagnosis; fresh-day and zero-shot tests |
| Vansh | `final_stock_LSTM_pipeline.ipynb` | Feature engineering with per-symbol isolation; five-class quantile-target study; Fischer-Krauss LSTM (daily and hourly); ranked-portfolio evaluation |

---

## Dataset

- **Source:** Yahoo Finance via `yfinance` (free, no API key) for daily / weekly / monthly bars; Alpaca for hourly bars; Databento (licensed, pulled with the $125 free credit) for limit order book depth (50 ms MBP-10 snapshots), minute bars, and index-futures feeds. The Databento data is too large for GitHub and is provided via the Drive link below.
- **Symbols:** SPY, NVDA, MU, TSLA, QQQ (QQQ is the order-book focus; TSLA is held out for zero-shot testing), plus GOOG, SLV/GLD, and VIX / 10Y yield / SMH as context in specific arms.
- **Date range:** daily 2016 to 2026 (weekly/monthly back to 1998), hourly ~last 2-4 years, order book ~3 weeks of 50 ms depth, minute bars 2023 to 2026.
- **Target:** next-period price direction (up/down) as the primary task, with a 3-class order-book direction (down/flat/up ~1 s ahead) and a 5-class magnitude/quintile variant.

---

## How to Run

Most notebooks are self-contained and run end-to-end in Google Colab with no API keys, since the daily / weekly / monthly / hourly data is pulled automatically from Yahoo Finance or committed to the repo:

1. Open the notebook of interest using the **Open in Colab** badge (or upload to Colab manually).
2. Run **Runtime > Run all**.

The order-book notebook (`Ron_LSTM.ipynb`) is different: its limit order book depth is licensed and too large for GitHub.

3. It ships with all cell outputs saved, so you can read every result without running it.
4. To re-run it, download the data from Drive and extract it into the repo's `data/` folder, keeping the same structure (`data/raw/`, `data/raw-fresh/`, `data/raw-july/`): https://drive.google.com/drive/folders/1i6TcSzkAvYy9U3T3dYqxxLmOYffc3gdi?usp=drive_link
5. A full re-run also needs the `databento` package and a GPU. A Databento API key (local `.env`) is only needed to pull fresh data, not to use the provided files.
---

## Project Status

Work in progress. Team members are actively developing their individual notebooks. A final merged analysis will be added here.

---

*CS207 · Section 5 · Team 4*
