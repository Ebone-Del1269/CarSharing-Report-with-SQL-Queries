# CarSharing Report with SQL Queries

## Project Overview

This project analyses CarSharing data for the year 2017 using SQL.

The analysis focuses on demand patterns, time-based demand, weather conditions, temperature categories, wind speed, and humidity.

The purpose of the analysis is to provide insights to help the company's marketing team understand demand patterns and other weather-related trends.

## Database Structure

The analysis was conducted using the following tables:

* `CarSharing_df` — main CarSharing dataset
* `temperature` — temperature categories and temperature information
* `adela_siwes project - weather` — weather conditions
* `adela_siwes project - time` — date, time, weekday, month, and season information

## Part 3 — SQL Queries

### 6(a) Highest Demand Rate in 2017
**SQL Query**
SELECT
   t.`timestamp`,
   c. demand 
FROM Carsharing_df AS c
JOIN `adela_siwes project - time` AS t 
   ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
ORDER BY c.demand DESC
LIMIT 1;

**Result** 
| Date and Time | Demand |
|---|---:|
| 2017-06-05 17:00:00 | 6.458338283

**Answer**: The highest demand rate recorded in 2017 was 6.458338283, occuring in the 15th of June 2017 at 5:00 pm

### 6(b) Highest and Lowest Average Demand

**SQL Query**

```sql
SELECT
    t.weekday_name,
    t.monthday_name,
    t.season,
    AVG(c.demand) AS average_demand
FROM carsharing_df AS c
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY
    t.weekday_name,
    t.monthday_name,
    t.season
ORDER BY average_demand DESC;
```

**Result**

|      Demand | Weekday | Month   | Season |
| ----------: | ------- | ------- | ------ |
| 4.997135079 | Sunday  | July    | Fall   |
| 3.050785778 | Monday  | January | Spring |

**Answer**

* Highest average demand: Sunday, July, Fall — **4.997135079**
* Lowest average demand: Monday, January, Spring — **3.050785778**

The weekdays identified here (**Sunday and Monday**) are used for the analysis in **6(c)**.

### 6(c) Average Demand by Hour for the Selected Weekdays

The weekdays identified in 6(b) were **Sunday and Monday**.

**SQL Query**

```sql
SELECT
    t.weekday_name,
    t.hour,
    AVG(c.demand) AS average_demand
FROM carsharing_df AS c
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
  AND t.weekday_name IN ('Sunday', 'Monday')
GROUP BY
    t.weekday_name,
    t.hour
ORDER BY
    t.weekday_name,
    average_demand DESC;
```

**Results**

#### Monday

|  Hour | Average Demand |
| ----: | -------------: |
| 13:00 |    5.643553886 |
| 12:00 |    5.621972408 |
| 14:00 |    5.554612764 |
| 15:00 |    5.515114817 |
| 16:00 |    5.503753106 |
| 11:00 |    5.437364717 |
| 17:00 |    5.399252221 |
| 10:00 |    5.223831412 |
| 18:00 |    5.215942911 |
| 19:00 |    4.990499880 |
| 20:00 |    4.726986121 |
| 09:00 |    4.638344517 |
| 21:00 |    4.464855593 |
| 00:00 |    4.230481522 |
| 22:00 |    4.188674635 |
| 01:00 |    3.976928867 |
| 08:00 |    3.934943629 |
| 23:00 |    3.799622394 |
| 02:00 |    3.768968806 |
| 03:00 |    3.074058279 |
| 07:00 |    3.007592707 |
| 06:00 |    2.002182357 |
| 05:00 |    1.743428605 |
| 04:00 |    1.659888488 |

#### Sunday

|  Hour | Average Demand |
| ----: | -------------: |
| 15:00 |    5.537925197 |
| 14:00 |    5.513702656 |
| 16:00 |    5.496274499 |
| 13:00 |    5.478758230 |
| 12:00 |    5.457459127 |
| 17:00 |    5.367634350 |
| 11:00 |    5.290915068 |
| 18:00 |    5.241211627 |
| 10:00 |    5.074545768 |
| 19:00 |    5.041802430 |
| 20:00 |    4.790666498 |
| 09:00 |    4.683176378 |
| 21:00 |    4.604221630 |
| 22:00 |    4.477619973 |
| 23:00 |    4.346754004 |
| 08:00 |    4.204916044 |
| 00:00 |    4.134974102 |
| 01:00 |    3.869853286 |
| 02:00 |    3.611232114 |
| 07:00 |    3.294024363 |
| 03:00 |    2.770262641 |
| 06:00 |    2.453218591 |
| 04:00 |    1.659227335 |
| 05:00 |    1.649176215 |

---

### 6(d) Weather Analysis

#### Temperature Category in 2017

**SQL Query**

```sql
SELECT
    temp.`temp category`,
    COUNT(*) AS occurrences
FROM carsharing_df AS c
JOIN temperature AS temp
    ON c.temp_code = temp.`temp code`
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY temp.`temp category`
ORDER BY occurrences DESC;
```

**Result**

| Temperature Category | Occurrences |
| -------------------- | ----------: |
| Mild                 |       3,282 |
| Hot                  |       1,436 |
| Cold                 |         704 |

**Answer:** The weather in 2017 was **mostly mild** based on the temperature categories.

#### Most Prevalent Weather Condition

**SQL Query**

```sql
SELECT
    w.weather,
    COUNT(*) AS occurrences
FROM carsharing_df AS c
JOIN `adela_siwes project - weather` AS w
    ON c.weather_code = w.weather_code
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY w.weather
ORDER BY occurrences DESC;
```

**Result**

| Weather Condition      | Occurrences |
| ---------------------- | ----------: |
| Clear or partly cloudy |       3,583 |
| Mist                   |       1,366 |
| Light snow or rain     |         473 |

**Answer:** The most prevalent weather condition in 2017 was **Clear or partly cloudy**.

#### Monthly Wind Speed

**SQL Query**

```sql
SELECT
    t.monthday_name AS month,
    AVG(c.windspeed) AS average_windspeed,
    MAX(c.windspeed) AS highest_windspeed,
    MIN(c.windspeed) AS lowest_windspeed
FROM carsharing_df AS c
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY t.monthday_name
ORDER BY MIN(t.`timestamp`);
```

**Result**

| Month     | Average Windspeed | Highest Windspeed | Lowest Windspeed |
| --------- | ----------------: | ----------------: | ---------------: |
| January   |           13.7481 |           39.0007 |                0 |
| February  |           15.5777 |           51.9987 |                0 |
| March     |           15.9749 |           40.9973 |                0 |
| April     |           15.8523 |           40.9973 |                0 |
| May       |           12.4274 |           40.9973 |                0 |
| June      |           11.8276 |           35.0008 |                0 |
| July      |           12.0158 |           56.9969 |                0 |
| August    |           12.4111 |           43.0006 |                0 |
| September |           11.5641 |           40.9973 |                0 |
| October   |           10.8921 |           36.9974 |                0 |
| November  |           12.1423 |           36.9974 |                0 |
| December  |           10.8365 |           43.0006 |                0 |


#### Monthly Humidity

**SQL Query**

```sql
SELECT
    t.monthday_name AS month,
    AVG(c.humidity) AS average_humidity,
    MAX(c.humidity) AS highest_humidity,
    MIN(c.humidity) AS lowest_humidity
FROM carsharing_df AS c
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY t.monthday_name
ORDER BY MIN(t.`timestamp`);
```

**Result**

| Month     | Average Humidity | Highest Humidity | Lowest Humidity |
| --------- | ---------------: | ---------------: | --------------: |
| January   |          56.3077 |              100 |              28 |
| February  |          53.5807 |              100 |               8 |
| March     |          55.9978 |              100 |               0 |
| April     |          66.2489 |              100 |              22 |
| May       |          71.3714 |              100 |              24 |
| June      |          58.3709 |              100 |              20 |
| July      |          60.2920 |               94 |              17 |
| August    |          62.1736 |               94 |              25 |
| September |          74.8404 |              100 |              42 |
| October   |          71.5714 |              100 |              29 |
| November  |          64.1692 |              100 |              27 |
| December  |          65.1806 |              100 |              26 |

#### Average Demand by Temperature Category

**SQL Query**

```sql
SELECT
    temp.`temp category` AS temperature_category,
    AVG(c.demand) AS average_demand
FROM carsharing_df AS c
JOIN temperature AS temp
    ON c.temp_code = temp.`temp code`
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY temp.`temp category`
ORDER BY average_demand DESC;
```

**Result**

| Temperature Category | Average Demand |
| -------------------- | -------------: |
| Hot                  |         4.8733 |
| Mild                 |         4.2619 |
| Cold                 |         3.2373 |

---

### 6(e) Analysis of the Month with the Highest Average Demand

**SQL Query**

```sql
SELECT
    t.monthday_name AS month,
    AVG(c.demand) AS average_demand
FROM carsharing_df AS c
JOIN `adela_siwes project - time` AS t
    ON c.id = t.id
WHERE YEAR(t.`timestamp`) = 2017
GROUP BY t.monthday_name
ORDER BY average_demand DESC;
```

**Result**

| Month     | Average Demand |
| --------- | -------------: |
| July      |         4.7877 |
| June      |         4.7239 |
| August    |         4.6423 |
| May       |         4.5716 |
| October   |         4.5624 |
| September |         4.5501 |
| November  |         4.4395 |
| December  |         4.2769 |
| April     |         4.0492 |
| March     |         3.7454 |
| February  |         3.6795 |
| January   |         3.3883 |

**Answer:** **July** had the highest average demand in 2017, with an average demand of **4.7877**.

#### July Temperature Categories

| Temperature Category | Occurrences |
| -------------------- | ----------: |
| Hot                  |         383 |
| Mild                 |          73 |
| Cold                 |           0 |

#### July Weather Conditions

| Weather Condition      | Occurrences |
| ---------------------- | ----------: |
| Clear or partly cloudy |         386 |
| Mist                   |          56 |
| Light snow or rain     |          14 |

#### July Wind Speed

| Measure | Wind Speed |
| ------- | ---------: |
| Average |    12.0158 |
| Highest |    56.9969 |
| Lowest  |          0 |

#### July Humidity

| Measure | Humidity |
| ------- | -------: |
| Average |  60.2920 |
| Highest |       94 |
| Lowest  |       17 |

### Google Drive Tables

The analysis tables will be stored in Google Drive and linked here.

### Conclusion

The analysis shows that demand varied across different times, weekdays, months, temperature categories, and weather conditions in 2017.

The highest individual demand was recorded on **15 June 2017 at 5:00 PM**, while **July** had the highest average monthly demand. The year was mostly **mild**, and **Clear or partly cloudy** was the most frequently recorded weather condition.

