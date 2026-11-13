# Seasonal Effects on City Bike Usage in Oslo

Final project for ACIT4510 Statistical Learning (OsloMet). It studies how season, day type (weekday vs weekend) and weather affect daily Oslo Bysykkel trips, and how well daily trips can be predicted for a new year.

## Research question

How do season and day type affect daily city bike usage in Oslo, and does the weekday–weekend difference depend on the season?

- Q1. Does daily usage differ between seasons, and how large is the effect?
- Q2. Is the weekday–weekend gap the same in every season (season × day-type interaction)?
- Q3. How much of the season effect is explained by weather (temperature, precipitation) and holidays?
- Q4. How well can daily usage be predicted for a new year (2026), and which model predicts best?

## Data

- Trips: [Oslo Bysykkel historical open data](https://oslobysykkel.no/en/open-data/historical), NLOD 2.0 licence. January 2024 – October 2026, about 3.3 million trips.
- The raw monthly CSV files are included in this repository: `Datas/2024/` (12 files), `Datas/2025/` (12 files) and `Datas/2026/` (January–October, 10 files). One row per trip.
- Weather: daily mean temperature and precipitation from the [Open-Meteo](https://open-meteo.com) historical archive API. It is fetched inside `data_preparation.ipynb` (no API key needed, internet connection required).
- Holidays: Norwegian public holidays from the Python `holidays` package (version 0.108).
- Timestamps in the raw files are UTC; they are converted to `Europe/Oslo` before the date, day type and season are derived.

## Notebooks (run in this order: A → D)

These are the same as appendices A–D of the report.

| | Notebook | What it does | Output |
|---|---|---|---|
| A | `assignment.ipynb` | Week 5 "Describe your data set" assignment: loads the 2025 trips, adds date, day type and season, and describes 2025 (time series, box plot, summary table, 17 May check). | `daily_2025.csv` |
| B | `data_preparation.ipynb` | Reads all years at once, counts trips per Oslo date, adds day type, season, year, the `closed` flag (17 May 2025, system closed), `is_holiday` and the Open-Meteo weather (left join on date). | `daily_allyears.csv` (1,034 days, 9 columns) |
| C | `modelling.ipynb` | Model 1: log OLS with season × day type on 2025, Type II ANOVA, partial η², residual checks, and the sensitivity analysis with and without 17 May. Model 2: + temperature, precipitation and holidays on 2024 + 2025, % effects, VIF, residuals and Durbin–Watson. | Figures 2–5 |
| D | `prediction.ipynb` | Naive baseline, OLS, regression tree, random forest and gradient boosting. Train on 2024, validate on 2025, retrain on 2024 + 2025 and test once on January–October 2026; metrics RMSE (log) and MAE. | Figures 1 and 6 |

## Main results

| | |
|---|---|
| Season effect (Model 1, 2025) | partial η² 0.75; weekends about half the weekday trips; no meaningful interaction on the log scale |
| Weather (Model 2) | R² 0.72 → 0.85; season sum of squares −84%; +7.8% trips per °C; holidays −55% |
| Prediction (2026 test, MAE in trips/day) | gradient boosting 595, random forest 602, OLS 751, naive 2,514 (an average 2026 day has about 3,566 trips) |

## Other files

- `daily_allyears.csv`: the daily data set used by the modelling and prediction notebooks.
- `daily_2025.csv`: 2025 daily counts from the Week 5 assignment.
- `figures/`: the six report figures (PNG):
  - `fig1_daily_trips.png`: daily trips 2024–2026 with a 7-day average
  - `fig2_boxplot_2025.png`: daily trips by season and day type, 2025
  - `fig3_resid_raw_vs_log.png`: Model 1 residuals, raw vs log scale
  - `fig4_resid_time_model1.png`: Model 1 residuals over time
  - `fig5_resid_model2.png`: Model 2 residuals vs fitted and over time
  - `fig6_test_2026.png`: observed vs predicted daily trips, 2026
- `FinalAssignment_Project_description (2).pdf`: the final project brief (report and oral examination requirements).

## Reproduce

Python 3.13 with pandas 2.3, NumPy 2.5, statsmodels 0.14, scikit-learn 1.7, matplotlib 3.10, seaborn 0.13 and holidays 0.108. Open the notebooks in the order A → D and run all cells; `random_state = 0` is used everywhere, so the results are reproducible.
