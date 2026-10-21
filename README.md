# Misdah
ML project predicting weekly Brent oil and gold prices using geopolitical conflict data (ACLED, GDELT), market indicators, and economic data in light of the Iran war.
# Predicting Oil Price in Light of the Iran War

A machine learning project (Group 2) that predicts the weekly average **Brent crude oil price** and **gold price** using geopolitical conflict data, media sentiment, financial markets, and economic indicators.

## Objective
Measure how geopolitical conflict in the Middle East affects oil and gold prices, and predict next week's average price.

## Data Sources
All data was aggregated into **weekly intervals** and merged into one dataset:

| Source | Content |
|---|---|
| **ACLED** | Weekly conflict events (battles, explosions, strategic events), fatalities, and events per country |
| **GDELT** | Global news volume, media sentiment, and geopolitical instability score (queried via Google BigQuery) |
| **Yahoo Finance** | Brent, WTI, gold, USD index, S&P 500 (SPY), energy stocks (XLE), defense stocks (ITA), OVX, VIX |
| **FRED / EIA** | Federal funds rate, US crude inventories, OPEC production |

## Methodology
1. **Data cleaning:** verified that dates from all sources align within a 7-day window, then merged them into a single `week_start` column.
2. **Feature selection:** used a correlation heatmap to drop redundant features (e.g. WTI, Total_Events), and merged related countries' events (Lebanon, Palestine, Jordan, Israel; Turkey and Syria).
3. **War intensity classification:** StandardScaler + K-Means (3 clusters) on conflict features to label each week as Low, Medium, or High war intensity.
4. **Feature engineering:** lag features for gold and Brent prices, plus the previous week's war intensity.
5. **Time-series split:** 80% train / 20% test, keeping time order to avoid looking into the future.

## Models
- **Gold price:** Ridge Regression (with StandardScaler)
- **Brent oil price:** Random Forest Regressor (100 trees)

Evaluation metrics: MAE, RMSE, and R².

## Results
<!-- Add your results from the notebook here, e.g.: -->
- Ridge (Gold): MAE = _, RMSE = _, R² = _
- Random Forest (Brent): MAE = _, RMSE = _, R² = _

## Tech Stack
Python, pandas, NumPy, scikit-learn, matplotlib, seaborn, yfinance

## Files
- `Group2_Iran_Oil.ipynb`: the full analysis and models
- `Merged.xlsx`: the merged weekly dataset (needed to run the notebook)

## How to Run
1. Open the notebook in Google Colab or Jupyter.
2. Upload `Merged.xlsx` and update the file path in the first data-loading cell.
3. Run all cells in order.

## Team
Group 2
