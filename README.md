# COVID-19 Analysis and 7-Day Forecast

Analysis of global and India COVID-19 trends with Plotly, plus a 7-day case forecast for India using Prophet.

## Dataset
Kaggle "Novel Corona Virus 2019 Dataset" (`covid_19_data.csv`): 306,429 rows, 22 Jan 2020 to 29 May 2021. Columns: date, province/state, country, confirmed, deaths, recovered. No duplicate rows. Province/State was blank for 78,103 rows, filled with "Unknown".

## Tools
Python, pandas, NumPy, Plotly, Prophet, Google Colab

## What I did
1. Cleaned the data (real dates, dropped unused columns, filled blank states)
2. Built world and India daily totals (the data is cumulative, so I grouped by date and summed)
3. Plotted world and India trends, recovery and death rates, daily new cases (with 7-day average) and the top 10 countries
4. Merged world and India tables on date to compare them
5. Forecast India's confirmed cases for 7 days with Prophet, checked on a 7-day holdout

## Findings
- India's daily new cases peaked at 414,188 on 6 May 2021 (second wave).
- By 29 May 2021, India had about 16.4% of world confirmed cases (27.9M of 170M).
- Top 3 by confirmed cases: US (33.25M), India (27.89M), Brazil (16.47M).
- The world "Recovered" line drops on 15 Dec 2020. The US alone fell by about 6.4M that day, so recovery data after that is unreliable. A world line without the US has no dip.
- India's death rate is very noisy until about June 2020, because early case counts were tiny.

## Forecast results (India, 7-day holdout)
| Model | MAE | RMSE | MAPE |
|---|---|---|---|
| Default Prophet | 5.22M | 5.22M | 19.1% |
| Tuned (changepoint_range=0.95, changepoint_prior_scale=0.5) | 1.17M | 1.22M | 4.27% |

The default model missed the second wave, likely because by default Prophet only lets the trend bend in the first 80% of the data. Allowing changepoints later and making the trend more flexible cut the error a lot (I changed both settings together, so I can't say which mattered more).

## Limitations
- I picked the tuned settings after seeing the holdout error, so 4.27% is optimistic.
- The final forecast (30 May to 5 Jun 2021) probably overshoots: the model was already above the real data at the end of the training period, and the wave was fading.
- Prophet's uncertainty band is very narrow, so it is overconfident.
- The forecast is for dates after the dataset ends, not for today.

## My takeaway
A forecast model may look fine on numbers but still can be wrong. so i checked where it failed and why

## How to run
Open `covid_19_analysis.ipynb` in Google Colab, run all cells, and upload `covid_19_data.csv` when asked.

