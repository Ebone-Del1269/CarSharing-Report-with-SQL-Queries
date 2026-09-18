# CarSharing Report with SQL Queries

## Project Overview

This project analyses CarSharing data for the year 2017 using SQL.

The analysis focuses on demand patterns, time-based demand, weather conditions, temperature categories, wind speed, and humidity.

The purpose of the analysis is to provide insights that can support the company's marketing team in understanding demand patterns and other weather-related trends.

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


### 6(c) Average Demand by Hour

### 6(d) Weather Analysis

### 6(e) Analysis of the Month with the Highest Average Demand

## Google Drive Tables

The analysis tables will be stored in Google Drive and linked here.

## Conclusion
