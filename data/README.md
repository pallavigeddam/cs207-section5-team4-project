# PG Datasets

 price data for the four-horizon direction study. All files pulled
once from Yahoo Finance via `build_dataset.py` with `auto_adjust=True`
(split and dividend adjusted). Frozen so every teammate and grader runs
on identical rows

## Files

| File | Contents | Rows | Range |
|------|----------|------|-------|
| `daily.csv` | Daily bars, core dataset | ~10k | 2016-06-20 to 2026-06-20 |
| `daily_long.csv` | Daily bars, extended history for weekly/monthly | ~40k | 1998-01 to 2026-06 |
| `hourly.csv` | Hourly bars (Yahoo's ~730-day limit) | ~19k | ~2024-07 to 2026-06 |
| `weekly.csv` | Weekly bars, resampled from daily | ~2k | 2016 to 2026 |
| `monthly.csv` | Monthly bars, resampled from daily | ~500 | 2016 to 2026 |

## Schema

Every file is long format with identical columns:

`timestamp, symbol, open, high, low, close, volume`

## Symbols

Stocks: SPY, NVDA, MU, TSLA
Market context: ^VIX (volatility), ^TNX (10-year Treasury yield), SMH (semiconductor ETF)

Note: TSLA history begins mid-2010; it contributes fewer rows at every horizon.

## Reproducing

Run `build_dataset.py` from the repo root to regenerate all files.
The script includes sanity checks that fail loudly on empty pulls,
duplicate bars, or unadjusted split data.
