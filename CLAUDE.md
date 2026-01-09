# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

```bash
# Render RMarkdown to PDF
Rscript -e "rmarkdown::render('Project1.Rmd', output_format='pdf_document')"

# Render RMarkdown to HTML
Rscript -e "rmarkdown::render('Project1.Rmd', output_format='html_document')"
```

## Required R Packages

```r
install.packages(c("fpp3", "readxl", "openxlsx", "imputeTS", "writexl"))
```

- `fpp3` - Core forecasting framework (includes tsibble, fable, feasts)
- `readxl` / `openxlsx` - Excel I/O and date conversion
- `imputeTS` - Time series imputation (`na_seadec()` for seasonal interpolation)
- `writexl` - Export forecasts to Excel

## Code Patterns

### Date Handling
Excel serial dates must be converted: `as.Date(DATE, origin = "1899-12-30")`

### Tsibble Conversion
```r
data %>% as_tsibble(index = DATE, key = ATM)
```

### Missing Value Imputation
Use seasonal-aware interpolation to preserve patterns:
```r
mutate(Cash = na_seadec(Cash, algorithm = "interpolation", find_frequency = TRUE))
```

### Outlier Handling
Replace outliers with NA, then impute:
```r
mutate(Cash = ifelse(Cash > threshold, NA, Cash)) %>%
  mutate(Cash = na_seadec(Cash, ...))
```

### Model Fitting
```r
model(
  snaive = SNAIVE(Cash),
  ets = ETS(Cash),
  arima = ARIMA(Cash),
  arima_trans = ARIMA(box_cox(Cash, lambda))
)
```

### Lambda Calculation
```r
lambda <- data %>% features(Cash, features = guerrero) %>% pull(lambda_guerrero)
```

## Seasonality Reference
- ATM data: period = 7 (weekly)
- Power data: period = 12 (yearly)
- Water data: period = 24 (hourly/daily)

## Output Files
Forecasts exported to: `atm[1-4]_forecasts.xlsx`, `power_forecasts.xlsx`, `water_forecasts.xlsx`
