# Global Weather Analysis & Forecasting


## Overview

The objective of this assessment was to analyze a global weather dataset, build forecasting models, generate advanced insights and demonstrate practical data science skills through end-to-end project execution.

The project includes data cleaning, exploratory data analysis, anomaly detection, machine learning forecasting, ensemble modeling and geographical climate insights.

---

## PM Accelerator Mission

PM Accelerator’s mission is to empower future product, AI and technology leaders through hands-on learning experiences, real-world projects and career acceleration opportunities that help candidates build impactful solutions and grow professionally.

---
# How to Run

1. Clone the repository
2. Install required libraries:

```bash
pip install -r requirements.txt
```

3. Open the notebook:

```bash
weather.ipynb
```

or run in **Google Colab**

---

## Project Structure

### 1. Exploratory Data Analysis (EDA)
- Dataset loading and initial inspection (shape, info, null values, summary statistics)
- Domain-based data cleaning : rows with physically impossible values (e.g. temperature outside −50°C to 60°C, wind speed > 200 kph) are removed; only 7 rows were filtered, confirming high dataset quality
- Distribution plots for temperature and precipitation (histograms, KDE curves, violin plots)
- Feature correlation heatmap across all numeric variables
- Top 15 hottest countries by average temperature
- Time-series trends for temperature and precipitation over time
- Scatter plot of temperature vs. precipitation

### 2. Baseline Time Series Forecasting
Three simple baseline models are built on daily average temperature (80/20 train/test split):

| Model | Description |
|---|---|
| Naive (Last Value) | Repeats the last training value for all future steps |
| Seasonal Naive | Repeats the last 7-day cycle |
| Rolling Mean | Forecasts using the 7-day trailing average |

All three produced negative R² scores, confirming that simple persistence-based methods are insufficient for complex global weather data.

### 3. Anomaly Detection
Three methods are applied to identify unusual weather observations:

- **Z-Score** - flags univariate temperature outliers beyond ±3 standard deviations
- **IQR (Interquartile Range)** - flags values outside 1.5×IQR from Q1/Q3
- **Isolation Forest** - multivariate anomaly detection across temperature, wind speed, pressure, humidity and precipitation (contamination = 1%)

Each method's anomalies are visualized as scatter plots of Temperature vs. Humidity. Isolation Forest is noted as the most suitable for complex, multi-dimensional weather data.

### 4. Advanced Forecasting
Five models are trained and compared using lag features (lag-1, lag-7, 7-day rolling mean):

| Model | Notes |
|---|---|
| ARIMA(5,1,0) | Classical statistical model |
| Prophet | Facebook's time series model with seasonality |
| Random Forest | Ensemble of decision trees |
| Gradient Boosting | Sequential boosting; best individual performer |
| XGBoost | Optimized gradient boosting |

**Key finding:** Tree-based ML models (Gradient Boosting, XGBoost, Random Forest) significantly outperform ARIMA and Prophet. Gradient Boosting achieves the lowest MAE and RMSE with the highest R². ARIMA and Prophet produce negative R² values due to the dataset's complexity and globally aggregated, multi-region nature.

An **Ensemble model** averaging Random Forest, Gradient Boosting, and XGBoost predictions is also evaluated. It provides stable performance but does not meaningfully exceed Gradient Boosting alone.

### 5. Unique Analyses

**Climate Analysis**
- Monthly global average temperature trend over time
- Top 10 rainiest countries by average precipitation
- Top 10 hottest countries by average temperature
- Global temperature distribution histogram

**Environmental Impact**
- Correlation heatmap between air quality indicators (PM2.5, PM10, CO, Ozone, NO₂) and weather variables
- Scatter plots of PM2.5 vs. humidity, wind speed, and temperature
- Finding: higher wind speeds generally reduce pollution via dispersion; humidity shows a moderate positive association with particulate accumulation

**Feature Importance**
- Tree-based importance from Gradient Boosting
- Permutation importance (10 repeats)
- Correlation-based feature analysis
- Lag features and weather variables (humidity, pressure) are consistently the most predictive

**Spatial Analysis**
- Latitude vs. temperature - clear decrease in temperature toward higher latitudes
- Longitude vs. temperature distribution
- Top 15 rainiest countries
- Geographic heatmaps of temperature and PM2.5 pollution plotted on longitude/latitude axes

**Geographical Patterns (Continental)**
- Countries mapped to continents (Africa, Asia, Europe, North America, South America, Oceania)
- Average temperature, precipitation, and PM2.5 by continent
- Tropical/equatorial regions (Africa, South America, Asia) show higher temperatures; industrialized regions show elevated PM2.5

---

## Requirements

```
numpy
pandas
matplotlib
seaborn
scikit-learn
scipy
statsmodels
prophet
xgboost
```

---

## Dataset

**GlobalWeatherRepository.csv** — a multi-country weather dataset with fields including:

- `temperature_celsius`, `humidity`, `wind_kph`, `pressure_mb`, `precip_mm`
- `air_quality_PM2.5`, `air_quality_PM10`, `air_quality_Carbon_Monoxide`, `air_quality_Ozone`, `air_quality_Nitrogen_dioxide`
- `latitude`, `longitude`, `country`, `last_updated`

---

## Key Takeaways

- The dataset is high quality with very few invalid records.
- Machine learning ensemble methods substantially outperform classical statistical forecasting for complex, globally aggregated weather data.
- Gradient Boosting is the single best-performing model.
- Air quality has a meaningful relationship with weather conditions, particularly wind speed and humidity.
- Global temperature patterns follow expected geographical distributions driven by latitude.
