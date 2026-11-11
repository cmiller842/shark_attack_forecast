# Forecasting Recorded Unprovoked Shark Attacks

This project examines how recorded unprovoked shark attacks worldwide changed over time and uses historical annual counts to forecast 2022–2031. It began as my final project for a time-series analysis course and was revised for this portfolio.

[Read the complete analysis and results](shark-attack-forecast.pdf).

## Project Overview

I investigated whether models based on historical counts could describe patterns in recorded unprovoked shark attacks and provide useful forecasts. I used records from **1916–2021** for model fitting and kept **2022** separate to compare the forecast with the recorded count.

The project demonstrates data preparation, visualization, statistical reasoning, model diagnostics, and communication of uncertainty. These skills apply to analyzing changing counts in areas such as demand planning and risk analysis.

## Data Source

The original dataset was downloaded for my course project as `global_shark_attacks.csv`. The download page is no longer available, but I retained the CSV. The original publisher and download URL are not documented here.

The data consists of individual incident records with fields such as year, incident type, country, and activity. This analysis uses `year` and `type`, keeps incidents classified as `Unprovoked`, and counts the records in each year. The 1916–2021 training period contains 106 consecutive annual counts. The recorded count for 2022 is 81.

## Methodology

1. **Prepare and explore the data.** Filter the incident records, aggregate them into annual counts, check year coverage, and plot the historical series.
2. **Fit a linear regression baseline.** Examine the overall trend and use a residual plot to identify patterns the straight-line model misses.
3. **Investigate temporal dependence.** Examine the ACF and PACF, fit an initial AR(1) model, and test its coefficient and residual autocorrelation.
4. **Check stationarity.** Apply the augmented Dickey–Fuller test, calculate first differences, and examine the resulting series.
5. **Select and assess an ARIMA model.** Use `auto.arima()` alongside the differenced ACF/PACF, fit ARIMA(1,1,0), and inspect coefficient tests, residual autocorrelation, and a normal Q–Q plot.
6. **Forecast and interpret.** Produce a ten-year forecast with prediction intervals and compare the 2022 forecast with the recorded count.

## Tools and Libraries

- **R and R Markdown** for analysis and the reproducible report.
- **dplyr** for filtering and aggregating records.
- **stats** for regression, ARIMA estimation, autocorrelation plots, and the Ljung–Box test.
- **lmtest** for coefficient tests.
- **tseries** for augmented Dickey–Fuller tests.
- **forecast** for automated ARIMA selection and forecasting.
- **rmarkdown and knitr** for rendering code, output, and explanations into the report.

## Key Findings

Recorded unprovoked attacks generally increased over the training period, with noticeable peaks and declines. The linear baseline explained about **73.4%** of the variation, but its residuals showed a curved pattern.

The initial AR(1) model left evidence of residual autocorrelation. Stationarity checks supported investigating first differencing, and `auto.arima()` selected **ARIMA(1,1,0)**. For this model, the Ljung–Box test found no significant residual autocorrelation through lag 10 (**p = 0.9578**). The Q–Q plot showed departures in the tails.

| 2022 forecast check | Recorded attacks |
| --- | ---: |
| Point forecast | 89.4 |
| Recorded count | 81 |
| Absolute forecast error | 8.4 |
| 80% prediction interval | 80.0–98.9 |
| 95% prediction interval | 74.9–103.9 |

The recorded count fell within both prediction intervals. Later point forecasts settle near **90 attacks annually**, while the intervals widen as uncertainty increases farther into the future. One year's forecast check provides limited evidence about accuracy across the full ten-year horizon.

## Limitations

- Changes in reporting and documentation could affect the number of recorded attacks over time.
- The dataset lacks measures of how many people entered the water or how long they stayed, so the analysis cannot estimate individual risk.
- The model uses historical counts alone. It does not establish which factors caused the observed changes.
- Larger fluctuations in later years and residual tail departures warrant caution when interpreting prediction intervals.
- Forecast validation covers only 2022, and the original dataset's download page is unavailable.

## Reproducibility

### Files

Keep these files together in the repository folder.

| File | Purpose |
| --- | --- |
| `shark-attack-forecast.Rmd` | Analysis code and explanations |
| `shark-attack-forecast.pdf` | Knitted report with figures and output |
| `global_shark_attacks.csv` | Incident dataset used by the report |
| `README.md` | Project summary and instructions |

### Run the analysis

1. Install R and RStudio, then download the repository files to one folder.
2. Install the required packages once in the R console.

   ```r
   install.packages(c("dplyr", "lmtest", "tseries", "forecast", "rmarkdown"))
   ```

3. Open `shark-attack-forecast.Rmd`. The import should use the relative path below so the report reads the CSV beside it.

   ```r
   attack_data <- read.csv("global_shark_attacks.csv", header = TRUE)
   ```

4. Use **Knit to PDF** in RStudio. PDF output requires a LaTeX installation. If needed, install TinyTeX once from the R console.

   ```r
   install.packages("tinytex")
   tinytex::install_tinytex()
   ```

   Alternatively, run this command from the repository folder in RStudio.

   ```r
   rmarkdown::render("shark-attack-forecast.Rmd")
   ```

5. Open the generated PDF to review the figures, diagnostics, forecast table, and interpretations.

## Future Work

I plan to evaluate forecasts across several withheld years to assess accuracy more thoroughly. Other possible extensions include comparing individual countries and investigating whether measures of beach attendance or ocean recreation improve forecasts.
