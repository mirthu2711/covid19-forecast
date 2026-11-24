# COVID-19 Forecasting with Johns Hopkins Data (PRCP-1023)

Forecasts India's daily new COVID-19 cases and deaths for the next 14 days using the Johns Hopkins CSSE time-series data, and turns the forecast into recommendations for health departments.

## Dataset
Johns Hopkins CSSE global time series (confirmed cases, deaths, recovered): 188 countries, 22 Jan to 21 Sep 2020. Values are cumulative, so they were converted to daily counts per country.

## What's inside
- **Data analysis report:** global overview, top-5 country comparison, India deep-dive (waves, weekly reporting pattern, deaths lagging cases)
- **Data cleaning:** negative daily values from downward revisions, an incomplete final day (21 Sep) removed, a one-day deaths backlog spike corrected
- **Forecasting:** naive, seasonal naive, moving average, Holt-Winters, SARIMA and LightGBM on lag features, with a chronological train/test split (last 14 days held out)
- **Rolling backtest:** six 14-day windows, because a single test window proved unreliable
- **Model comparison, challenges report and recommendations for the health department**

## Results (India, daily new cases)
| Model | Backtest mean MAE (6 windows) |
|---|---|
| **SARIMA(1,1,1)(1,0,1,7)** | **5,865** |
| Holt-Winters | 7,033 |
| Naive (last value) | 9,600 |
| 7-day moving average | 10,376 |
| LightGBM (lag features) | 14,811 |

SARIMA is recommended for production. The naive model looked best on the single final test window (MAE 4,782) but ranked third once errors were averaged over six windows.

## Forecast (21 Sep to 4 Oct 2020)
- About 90k to 100k new cases per day (roughly 1.3 million over 14 days), a plateau rather than a decline
- About 1,230 deaths per day (roughly 17,200 over 14 days)

## Key findings
- The US, India and Brazil held about half of global cases on 21 Sep 2020.
- India's cases were still rising in September, while the US, Brazil and Colombia had already peaked.
- Daily counts follow a weekly reporting cycle, so decisions should use 7-day averages.
- Tree models cannot predict above the range seen in training, so LightGBM fared worst on a series still hitting new highs.

## Files
- `covid19-forecasting.ipynb`: full analysis and models
- `covid19-forecasting.html`: exported notebook
- `PRCP-1023-JohnsHopkinsCovid19.pdf`: problem statement
- `time_series_covid19_*_global.csv`: data files
- `requirements.txt`

## Limitations
Only the first 8 months of data are covered, the forecast is national (not state-level), and it assumes no change in testing, restrictions or reporting.

## Tech
Python, pandas, statsmodels, scikit-learn, LightGBM, matplotlib