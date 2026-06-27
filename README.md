# Stock Movement Classification — CS207 Section 5, Team 4

Predicting next-day price direction for **SPY, NVDA, MU, and TSLA** as a 5-class quantile problem, using 10 years of daily OHLCV data from Yahoo Finance, 25 stationary technical features, a multinomial logistic regression baseline, and a TensorFlow LSTM.

---

## Problem Framing

| | |
|---|---|
| **Task** | Multi-class classification of next-day return quintile (Worst 20% / 20–40% / 40–60% / 60–80% / Best 20%) |
| **Input** | 25 stationary technical indicators computed from daily OHLCV bars |
| **Output** | One of 5 quintile labels for the next trading day |
| **Labeled data** | Next-day forward return, quintile-binned per symbol; label is never leaked into features |
| **Train/val/test split** | Chronological: 70% train · 15% val · 15% test (no shuffling) |
| **Evaluation metric** | Macro F1 (all 5 classes weighted equally) and accuracy |
| **Baseline** | Majority-class (most-frequent) classifier |

---

## Dataset

- **Source:** Yahoo Finance via `yfinance` (no API key required)
- **Symbols:** SPY, NVDA, MU, TSLA
- **Date range:** 2016-06-20 to 2026-06-20 (~10 years of daily bars)
- **Train rows (daily):** ~6,240 | **Test rows:** ~1,504

---

## Features (25 stationary indicators)

All features are stationary — returns, ratios, and bounded indicators — so the scaler trained on 2016 prices generalises to 2025 prices.

| Group | Features |
|---|---|
| Returns | ret_1, ret_lag1, ret_lag2, ret_lag3 |
| Momentum | mom_10, mom_20, mom_50, mom_200 |
| Price-to-MA distance | price_to_ma10, price_to_ma20, price_to_ma50, price_to_ma200 |
| MA crossover | ma_cross (50/200 golden/death cross signal) |
| Oscillators | rsi_14, macd, macd_hist, bb_pct |
| Volatility | volatility_20 |
| Volume | vol_ratio20, vol_ratio50 |
| Extra indicators | atr_14, adx_14, mfi_14, cmf_20, gap |

Started with 32 candidates; 7 removed as redundant via correlation analysis.

---

## Models

### 1. Majority-Class Baseline
Predicts the most frequent class for every sample. Sets the lower bound for both accuracy and Macro F1.

### 2. Multinomial Logistic Regression
Trained on the last day of each sliding window (flat feature vector). Serves as an interpretable linear baseline.

### 3. TensorFlow LSTM
- Architecture: LSTM(64) -> Dropout -> Dense(32) -> Dense(5, softmax)
- Input window: 20 trading days (~1 month) of the 25 features
- Training: up to 30 epochs with early stopping, batch size 64
- GPU runtime recommended

---

## Results

All three models are evaluated on the **same held-out test sequences**.

| Model | Accuracy | Macro F1 |
|---|---|---|
| Majority baseline | 0.215 | 0.071 |
| Logistic Regression | 0.229 | 0.215 |
| LSTM | 0.231 | 0.212 |

Both learned models beat the baseline by ~3x on Macro F1. Accuracy gains are modest because the baseline already scores 21.5% by always predicting the plurality class in a balanced 5-class problem.

**Timeframe comparison (Logistic Regression):**

| Timeframe | Train rows | Test rows | Majority F1 | Logistic F1 |
|---|---|---|---|---|
| Daily | 6,240 | 1,504 | 0.071 | 0.216 |
| Weekly | 660 | 312 | 0.062 | 0.201 |

The lift over the baseline grows at longer timeframes, suggesting technical features carry more signal at a weekly horizon.

---

## Repo Structure

```
cs207-section5-team4-project/
├── Pallavi_Stock_Classification_Full_YahooDataSet.ipynb   # Full pipeline: data, features, EDA, models, evaluation
└── README.md
```

---

## How to Run

1. Open the notebook in Google Colab using the **Open in Colab** badge at the top of the notebook.
2. Run the setup cell (Cell 1) once per session to install `yfinance`.
3. Select **Runtime > Run all**. A GPU runtime is recommended for the LSTM training step.
4. All configuration (symbols, date range, number of classes, LSTM knobs) lives in the single setup cell at the top.

---

## Next Steps

1. Add Random Forest and XGBoost on the same 25 features, compared across all three timeframes.
2. Walk-forward (expanding-window) validation so every regime is scored fully out-of-sample.
3. Bidirectional LSTM layer and a light hyperparameter sweep (window 20 vs 30, LSTM units, dropout).
4. Add market-wide context features (VIX, rate proxy, credit proxy), joined on the same date.
5. Merge with teammates' work.

---

## Team

CS207 · Section 5 · Team 4
