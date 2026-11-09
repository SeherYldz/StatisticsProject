# Seasonal Effects on City Bike Usage in Oslo

Final project for Statistical Learning (OsloMet). It studies how season, day type (weekday vs weekend) and weather affect daily Oslo Bysykkel trips, and how well daily trips can be predicted for a new year.

## Research question

How do season and day type affect daily city bike usage in Oslo, and does the weekday–weekend difference depend on the season?

- Q1. Does daily usage differ between seasons, and how large is the effect?
- Q2. Is the weekday–weekend gap the same in every season (season × day-type interaction)?
- Q3. How much of the season effect is explained by weather (temperature, precipitation) and holidays?
- Q4. How well can daily usage be predicted for a new year (2026), and which model predicts best?

## Data

- Trips: [Oslo Bysykkel historical open data](https://oslobysykkel.no/en/open-data/historical), NLOD 2.0 licence. January 2024 – October 2026, about 3.3 million trips.
- Weather: daily mean temperature and precipitation from the [Open-Meteo](https://open-meteo.com) historical archive (fetched in `data_preparation.ipynb`, no API key needed).
- Holidays: Norwegian public holidays from the Python `holidays` package.
- The raw CSV files are **not** stored in this repository. Download the monthly files and save them as `Datas/2024/*.csv`, `Datas/2025/*.csv` and `Datas/2026/*.csv`.
- Timestamps in the raw files are UTC; they are converted to `Europe/Oslo` before the date, day type and season are derived.

## Notebooks (run in this order)

1. `assignment.ipynb`:  Describing data set assignment (2025 only, descriptive analysis).
2. `data_preparation.ipynb`: reads all years, counts trips per day, adds day type, season, the `closed` flag (17 May 2025), `is_holiday` and weather, and saves `daily_allyears.csv` (1,034 days).
3. `modelling.ipynb`: Model 1 (log OLS, season × day type, 2025) and Model 2 (+ temperature, precipitation and holidays, 2024 + 2025), with ANOVA, effect sizes, VIF and residual checks. The last cell saves Figures 2–5.
4. `prediction.ipynb`: OLS, regression tree, random forest and gradient boosting; train on 2024, validate on 2025, retrain on 2024 + 2025 and test once on 2026. Saves Figures 1 and 6.

## Main results

| | |
|---|---|
| Season effect (Model 1, 2025) | partial η² 0.75; weekends about half the weekday trips; no meaningful interaction on the log scale |
| Weather (Model 2) | R² 0.72 → 0.85; season sum of squares −84%; +7.8% trips per °C; holidays −55% |
| Prediction (2026 test, MAE in trips/day) | gradient boosting 595, random forest 602, OLS 751, naive 2,514 |

## Other files

- `daily_allyears.csv`: the daily data set used by the modelling and prediction notebooks.
- `daily_2025.csv`: 2025 daily counts from the Week 5 assignment.
- `figures/`: the report figures (PNG).
- `DescribeYourDataSet-1.pdf`: ma assignment brief.

Software: Python 3.13, pandas 2.3, NumPy 2.5, statsmodels 0.14, scikit-learn 1.7, matplotlib 3.10, seaborn 0.13.
