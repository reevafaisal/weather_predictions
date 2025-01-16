## Skies Unveiled: Patterns, Predictions, and Anomalies in Weather Data

How accurately can a SARIMA model forecast temperature values while accounting for anomalies in the data, and how robust is the model's performance in identifying and predicting anomalous patterns in the time series?

### Introduction

The dataset, [Global Weather Repository](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository/code), provides comprehensive meteorological data collected from various locations 
worldwide. It offers insights into various weather parameters, including temperature, precipitation, and humidity, 
enabling detailed global climate patterns and trend analysis. This dataset is valuable for understanding
weather-related phenomena and their implications on environmental and societal systems.

<p>
Before any cleaning, the dataset contains 46967 rows.
The following columns are relevant to the analysis:
</p>

| Column Name                      | Description                                                              |
|----------------------------------|--------------------------------------------------------------------------|
| country                          | The name of the country where the weather data was recorded.             |
| location_name                    | The specific location name for the weather data.                         |
| latitude                         | The latitude of the location in decimal degrees.                         |
| longitude                        | The longitude of the location in decimal degrees.                        |
| timezone                         | The timezone of the location.                                            |
| last_updated                     | The last update time in a human-readable format.                         |
| temperature_celsius              | The temperature at the location in degrees Celsius.                     |
| condition_text                   | A description of the current weather condition.                         |
| wind_kph                         | The wind speed in kilometers per hour.                                   |
| wind_degree                      | The direction of the wind in degrees.                                    |
| wind_direction                   | The cardinal direction of the wind.                                      |
| pressure_mb                      | The atmospheric pressure at the location in millibars.                  |
| precip_mm                        | The amount of precipitation in millimeters.                             |
| humidity                         | The percentage of atmospheric moisture at the location.                 |
| cloud                            | The percentage of cloud cover at the location.                          |
| feels_like_celsius               | The perceived temperature at the location in degrees Celsius.           |
| visibility_km                    | The visibility distance at the location in kilometers.                  |
| uv_index                         | The UV index indicates the strength of ultraviolet radiation.           |
| gust_kph                         | The speed of wind gusts in kilometers per hour.                         |
| air_quality_Carbon_Monoxide      | The concentration of carbon monoxide in the air.                        |
| air_quality_Ozone                | The concentration of ozone in the air.                                  |
| air_quality_Nitrogen_dioxide     | The concentration of nitrogen dioxide in the air.                       |
| air_quality_Sulphur_dioxide      | The concentration of sulphur dioxide in the air.                        |
| air_quality_PM2.5                | The concentration of particulate matter smaller than 2.5 micrometers.   |
| air_quality_PM10                 | The concentration of particulate matter smaller than 10 micrometers.    |
| air_quality_us-epa-index         | The air quality index as per the US EPA standards.                      |
| air_quality_gb-defra-index       | The air quality index as per the UK DEFRA standards.                    |
| moon_illumination                | The percentage of the moon that is illuminated.                         |


### Data Cleaning 
- Removed columns that were either irrelevant to the analysis or provided redundant information, such as duplicate measurements in different units or unnecessary time-related columns.
- Transformed the `last_updated` column into a datetime format for consistency and easier time-based analysis.

| country     | location_name    |   latitude |   longitude | timezone       | last_updated        |   temperature_celsius | condition_text   |   wind_kph |   wind_degree | wind_direction   |   pressure_mb |   precip_mm |   humidity |   cloud |   feels_like_celsius |   visibility_km |   uv_index |   gust_kph |   air_quality_Carbon_Monoxide |   air_quality_Ozone |   air_quality_Nitrogen_dioxide |   air_quality_Sulphur_dioxide |   air_quality_PM2.5 |   air_quality_PM10 |   air_quality_us-epa-index |   air_quality_gb-defra-index |   moon_illumination |   Temperature_Z |   Precipitation_Z | Anomaly_Temperature_Z   | Anomaly_Precipitation_Z   | Anomaly_Temperature_IQR   | Anomaly_Precipitation_IQR   |
|:------------|:-----------------|-----------:|------------:|:---------------|:--------------------|----------------------:|:-----------------|-----------:|--------------:|:-----------------|--------------:|------------:|-----------:|--------:|---------------------:|----------------:|-----------:|-----------:|------------------------------:|--------------------:|-------------------------------:|------------------------------:|--------------------:|-------------------:|---------------------------:|-----------------------------:|--------------------:|----------------:|------------------:|:------------------------|:--------------------------|:--------------------------|:----------------------------|
| Afghanistan | Kabul            |      34.52 |       69.18 | Asia/Kabul     | 2024-05-16 13:15:00 |                  26.6 | Partly Cloudy    |       13.3 |           338 | NNW              |          1012 |         0   |         24 |      30 |                 25.3 |              10 |          7 |       15.3 |                         277   |               103   |                            1.1 |                           0.2 |                 8.4 |               26.6 |                          1 |                            1 |                  55 |       0.370189  |        -0.231145  | False                   | False                     | False                     | False                       |
| Albania     | Tirana           |      41.33 |       19.82 | Europe/Tirane  | 2024-05-16 10:45:00 |                  19   | Partly cloudy    |       11.2 |           320 | NW               |          1012 |         0.1 |         94 |      75 |                 19   |              10 |          5 |       18.4 |                         193.6 |                97.3 |                            0.9 |                           0.1 |                 1.1 |                2   |                          1 |                            1 |                  55 |      -0.474459  |        -0.0743989 | False                   | False                     | False                     | True                        |
| Algeria     | Algiers          |      36.76 |        3.05 | Africa/Algiers | 2024-05-16 09:45:00 |                  23   | Sunny            |       15.1 |           280 | W                |          1011 |         0   |         29 |       0 |                 24.6 |              10 |          5 |       22.3 |                         540.7 |                12.2 |                           65.1 |                          13.4 |                10.4 |               18.4 |                          1 |                            1 |                  55 |      -0.0299074 |        -0.231145  | False                   | False                     | False                     | False                       |
| Andorra     | Andorra La Vella |      42.5  |        1.52 | Europe/Andorra | 2024-05-16 10:45:00 |                   6.3 | Light drizzle    |       11.9 |           215 | SW               |          1007 |         0.3 |         61 |     100 |                  3.8 |               2 |          2 |       13.7 |                         170.2 |                64.4 |                            1.6 |                           0.2 |                 0.7 |                0.9 |                          1 |                            1 |                  55 |      -1.88591   |         0.239093  | False                   | False                     | False                     | True                        |
| Angola      | Luanda           |      -8.84 |       13.23 | Africa/Luanda  | 2024-05-16 09:45:00 |                  26   | Partly cloudy    |       13   |           150 | SSE              |          1011 |         0   |         89 |      50 |                 28.7 |              10 |          8 |       20.2 |                        2964   |                19   |                           72.7 |                          31.5 |               183.4 |              262.3 |                          5 |                           10 |                  55 |       0.303506  |        -0.231145  | False                   | False                     | False                     | False                       |

### Advanced Exploratory Data Analysis

#### Statistical Anomaly Detection

Moving forward, I am choosing to focus on **precipitation** and **temperature** as key variables for statistical anomaly detection. Temperature and precipitation offer a meaningful basis for comparison across different locations and periods, making them ideal for detecting and understanding statistical anomalies in weather data. This focus will enable a targeted and practical approach to uncovering significant outliers and trends.

##### Z-Score Method

We will first use the Z-Score and IQR methods to flag statistical anomalies.

<div style="margin-bottom: 5px;">
  <iframe src="assets/temp_comp.html" width="700" height="400px" frameborder="0" scrolling="yes" style="transform: translateX(-50px);margin-bottom: 5px;"></iframe>
</div>

**Z-Score Method:**
- Total number of outliers predicted: 258.
- Values that exceed a threshold of |z| > 3 are flagged as outliers.

**IQR Method:**
- Total number of outliers predicted: 1527.
-Values beyond 1.5 * IQR from the first or third quartile are flagged as outliers.

The vast difference in the number of outliers predicted can be due to the fact that the Z-Score method assumes that the data is normally distributed while the IQR method makes no such assumption. It is thus important to assess the skewness of the data for more accurate predictions.



**Data-Driven Anomaly Detection:**







