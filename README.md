# Misdah
ML project predicting weekly Brent oil and gold prices using geopolitical conflict data (ACLED, GDELT), market indicators, and economic data in light of the Iran war.
# Predicting Oil Price in Light of the Iran War

A machine learning project (Group 2) that predicts the weekly average **Brent crude oil price** and **gold price** using geopolitical conflict data, media sentiment, financial markets, and economic indicators.

## Objective
Measure how geopolitical conflict in the Middle East affects oil and gold prices, and predict next week's average price.

## Data Sources
All data was aggregated into **weekly intervals** and merged into one dataset:

## Dataset

**File:** `Merged.xlsx`
**Granularity:** Weekly (one row per week, identified by `week_start`)
**Period:** 2022 to 2026 <!-- adjust to the exact first and last week in your data -->
**Size:** _ rows × _ columns <!-- fill from df.shape -->

The dataset merges four sources into a single weekly table. Daily data was averaged per week so all sources share the same time unit.

### Sources and Features

**1. ACLED (conflict events)**
| Feature | Description |
|---|---|
| `Fatalities` | Number of fatalities in the week |
| `Battles_Events`, `Explosions_Events`, `Strategic_Events` | Weekly counts by conflict type |
| `<Country>_Events` | Events recorded per country (Bahrain to Yemen) |

**2. GDELT (global media)**
| Feature | Description |
|---|---|
| `average_media_sentiment` | Average tone of news coverage |
| `geopolitical_instability_score` | Calculated regional instability score |
| `media_panic_amplification` | Media focus on crisis |
| `kinetic_warfare`, `economic_coercion`, `diplomatic_threats` | Volume of each conflict type found in news |

**3. Yahoo Finance (markets)**
| Feature | Description |
|---|---|
| `target_brent_price` | Brent crude price (**main target**) |
| `gold_price` | Gold price (**second target**) |
| `usd_index` | U.S. Dollar index |
| `sp500_etf` (SPY) | U.S. stock market |
| `energy_stocks` (XLE) | Energy companies ETF |
| `defense_stocks` (ITA) | Aerospace and defense ETF |
| `oil_volatility` (OVX) | Expected 30-day oil volatility |
| `market_volatility` (VIX) | Market fear index |

**4. FRED / EIA (economy and supply)**
| Feature | Description |
|---|---|
| `US_Crude_Inventories_mb` | U.S. crude inventories (million barrels) |

### Preprocessing
- Verified all date columns align within a 7-day window, then merged them into one `week_start` column.
- Dropped redundant or weakly correlated features: `wti_price`, `Total_Events`, `total_geopolitical_event_volume`, `FEDFUNDS`, `OPEC_Production_mbpd`.
- Combined related countries: Lebanon + Palestine + Jordan + Israel, and Turkey + Syria.

### Engineered Features
| Feature | Description |
|---|---|
| `War_Intensity` | Low / Medium / High, from K-Means (3 clusters) on conflict features |
| `War_Intensity_Lag1` | Previous week's war intensity |
| `Gold_Lag_1`, `Gold_Lag_2`, `Gold_Lag_4` | Gold price 1, 2, and 4 weeks earlier |
| `Brent_Lag_1`, `Brent_Lag_2` | Brent price 1 and 2 weeks earlier |

### Data Sources
- ACLED: https://acleddata.com
- GDELT (via Google BigQuery): https://www.gdeltproject.org
- Yahoo Finance (via `yfinance`)
- FRED: https://fred.stlouisfed.org
- EIA: https://www.eia.gov
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
