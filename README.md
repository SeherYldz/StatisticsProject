# Seasonal Effect On Bicycle Usage In Oslo City

Statistical Learning (OsloMet), "Describe your data set" assignment. The same data set will be used for the final report.

## Research question

1. Does Oslo city bike usage differ significantly across seasons?
2. Is the weekday vs weekend difference constant across seasons, or does it change with the season (season × day-type interaction)?

## Data

- Source: [Oslo Bysykkel open data](https://oslobysykkel.no/en/open-data/historical), licensed under NLOD 2.0 (Norwegian License for Open Government Data).
- The raw CSV files are **not** stored in this repository. Download the 12 monthly files for 2025 and save them as `Datas/01.csv` … `Datas/12.csv`.
- Timestamps in the files are UTC. The notebook converts them to `Europe/Oslo` time before deriving the date, day type and season.

## Files

- `assignment.ipynb`: the working notebook. It loads and combines the data, converts it to Oslo time, and adds the `date`, `day_type` and `season` columns.
- `DescribeYourDataSet-1.pdf`: the assignment brief.
