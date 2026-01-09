# ATM Demand Forecasting in R

A comprehensive time series analysis and forecasting project demonstrating predictive analytics across three real-world domains: **ATM cash withdrawals**, **residential power consumption**, and **water flow management**.

## Project Highlights

- **Multi-domain forecasting**: Applied consistent methodology across financial (ATM), energy (power), and utility (water) sectors
- **Model comparison**: Evaluated SNAIVE, ETS, and ARIMA models with Box-Cox transformations
- **Production-ready outputs**: Generated 31-day ATM forecasts, 12-month power forecasts, and 7-day water flow predictions

## Datasets Analyzed

| Domain | Data | Frequency | Forecast Horizon |
|--------|------|-----------|------------------|
| Financial | 4 ATM machines cash withdrawals | Daily | 31 days |
| Energy | Residential customer power load | Monthly | 12 months |
| Utility | Water pipe flow (2 pipes) | Hourly | 168 hours (1 week) |

## Technical Approach

### Data Preprocessing
- **Date conversion**: Transformed Excel serial dates to proper datetime formats
- **Missing value imputation**: Used seasonal decomposition interpolation (`na_seadec`) to preserve time series patterns
- **Outlier handling**: Identified and replaced extreme values using domain-appropriate thresholds
- **Data aggregation**: Combined multiple pipe sensors into unified hourly flow measurements

### Exploratory Analysis
- Time series decomposition (STL) to identify trend, seasonality, and remainder components
- ACF/PACF analysis for determining appropriate model orders
- Guerrero's method for optimal Box-Cox transformation parameters
- Stationarity assessment through visual inspection and differencing tests

### Forecasting Models

**Seasonal ARIMA** - Primary model for most datasets
- ATM1: ARIMA(0,0,2)(0,1,1)[7] with weekly seasonality
- ATM2: ARIMA(0,0,2)(1,1,1)[7] with weekly seasonality
- ATM4: ARIMA(0,0,1)(2,0,0)[7] with weekly seasonality
- Power: ARIMA(0,0,1) with yearly seasonality
- Water: ARIMA(0,1,1)(0,0,1)[24] with daily seasonality

**ETS (Error, Trend, Seasonality)** - Used where ARIMA underperformed
- ATM3: ETS model for sparse data with limited non-zero observations
- ATM4: ETS with Box-Cox transformation for final forecasts

**Seasonal Naive** - Baseline and alternative
- Water flow: SNAIVE captured patterns better than complex models

### Model Selection Criteria
- AIC, AICc, and BIC for model comparison
- Residual diagnostics (normality, ACF of residuals)
- Visual forecast validation against historical patterns

## Key Findings

- **Weekly seasonality** dominates ATM withdrawal patterns (7-day cycle)
- **Yearly seasonality** drives residential power consumption (12-month cycle)
- **Daily seasonality** characterizes water flow patterns (24-hour cycle)
- Box-Cox transformations improved model performance when variance increased over time
- Simpler models (SNAIVE) sometimes outperform complex models on noisy data

## Technologies Used

- **R** - Statistical computing
- **fpp3** - Forecasting framework (tsibble, fable, feasts)
- **tidyverse** - Data manipulation and visualization
- **imputeTS** - Time series imputation
- **RMarkdown** - Reproducible reporting

## Repository Structure

```
├── Project1.Rmd          # Main analysis (RMarkdown)
├── Project1.pdf          # Rendered report
├── ATM624Data.xlsx       # ATM withdrawal data
├── ResidentialCustomerForecastLoad-624.xlsx  # Power data
├── Waterflow_Pipe1.xlsx  # Water flow sensor 1
├── Waterflow_Pipe2.xlsx  # Water flow sensor 2
├── *_forecasts.xlsx      # Generated forecast outputs
└── README.md
```

## How to Run

```r
# Install required packages
install.packages(c("fpp3", "readxl", "openxlsx", "imputeTS", "writexl"))

# Render the analysis
rmarkdown::render("Project1.Rmd")
```

## Author

Joey Bochnik

## License

This project is available for educational and portfolio purposes.
