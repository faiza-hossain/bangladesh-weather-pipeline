# Bangladesh Decadal Weather Pipeline (2016 - 2025)

An automated data engineering pipeline that extracts, processes, and archives 10 years of continuous, hourly historical weather parameters for 4 major divisions in Bangladesh: **Dhaka, Chittagong, Sylhet and Rajshahi**.

**Kaggle Dataset Link:** (https://www.kaggle.com/datasets/faizahossain/bangladesh-weather-dataset2016-2025)  
**Usability Score:** 9.41/10

---

## Project Overview
This project contains an end-to-end data pipeline written in Python that programmatically extracts meteorological reanalysis data. The resulting dataset spans exactly a decade (Jan 1, 2016 – Dec 31, 2025), delivering **350,880 rows** of continuous hourly tracking. It is designed for time-series forecasting, climate change analysis, and regional environmental modeling.

## Dataset Architecture
The compiled dataset (`bangladesh_weather_2016_2025.csv`) tracks 10 critical parameters:

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `time` | Datetime (ISO 8601) | Hourly observation timestamp (UTC) |
| `temperature_2m` | Float | Air temperature at 2 meters above ground (°C) |
| `relative_humidity_2m` | Integer | Relative humidity at 2 meters above ground (%) |
| `rain` | Float | Liquid precipitation fallen during the hour (mm) |
| `pressure_msl` | Float | Atmospheric pressure adjusted to Mean Sea Level (hPa) |
| `cloud_cover` | Integer | Total cloud cover area fraction (%) |
| `wind_speed_100m` | Float | Wind speed at 100 meters above ground (km/h) |
| `wind_direction_100m` | Integer | Wind direction at 100 meters above ground (0-360°) |
| `soil_temperature_100_to_255cm` | Float | Deep-layer underground soil temperature (°C) |
| `city` | String | Target division (Dhaka, Chittagong, Sylhet, Rajshahi) |

---

## Data Pipeline & Provenance
* **Source Provider:** European Centre for Medium-Range Weather Forecasts (ECMWF) ERA5 Reanalysis Model, served via the **Open-Meteo Historical Weather API**.
* **Collection Methodology:** A custom Python pipeline loops through the exact latitude and longitude coordinates of the target administrative divisions. 
* **Data Engineering:** Textual timestamps are converted to native datetime configurations, geographic labels are appended dynamically, and the independent arrays are vertically concatenated into a single, optimized static layout using Pandas.

---

## Quick Starter Analysis
You can easily load and visualize the annual warming trends across the divisions using this minimal Python snippet:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df = pd.read_csv('bangladesh_weather_2016_2025.csv')
df['time'] = pd.to_datetime(df['time'])
df['year'] = df['time'].dt.year
annual_trends = df.groupby(['year','city'])['temperature_2m'].mean().reset_index()
plt.figure(figsize=(10,5))
sns.lineplot(data=annual_trends,x='year',y='temperature_2m',hue='city',marker='o')
plt.title('Average Annual Temperature Trends in Bangladesh(2016-2025)')
plt.ylabel('Mean Temperature (°C)')
plt.grid(True,alpha=0.3)
plt.show()
