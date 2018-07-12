# Memo for Google BigQuery

Useful commands/scripts for use on Google Cloud Platform.



## Definition

BigQuery is the PetaBytes-scale datawarehouse on GCP (for <u>**immutable**</u> **very-large** datasets)



## Start web console for BigQuery

Use one of these:

* https://bigquery.cloud.google.com
* https://console.cloud.google.com/ then from the 'hamburger' menu, look for `BigQuery` under  the `Big Data` section.


## Query large datasets in seconds (Examples)


```sql
#standardsql
SELECT
  nppes_provider_state AS state,
  ROUND(SUM(total_claim_count) / 1e6) AS total_claim_count_millions
FROM
  `bigquery-public-data.medicare.part_d_prescriber_2014`
GROUP BY
  state
ORDER BY
  total_claim_count_million DESC
LIMIT
  5;
```

### Simple Query

```sql
SELECT
  airline,
  date,
  departure_delay
FROM
  `bigquery-samples.airline_ontime_data.flights`
WHERE
  departure_delay > 0
  AND departure_airport = 'LGA'
LIMIT
  100
```


### Aggregate and Boolean functions

```sql
SELECT
  airline,
  COUNT(departure_delay)
FROM
   `bigquery-samples.airline_ontime_data.flights`
WHERE
  departure_airport = 'LGA'
  AND date = '2008-05-13'
GROUP BY
  airline
ORDER BY airline
```


```sql
SELECT
  airline,
  COUNT(departure_delay)
FROM
   `bigquery-samples.airline_ontime_data.flights`
WHERE
  departure_delay > 0 AND
  departure_airport = 'LGA'
  AND date = '2008-05-13'
GROUP BY
  airline
ORDER BY airline
```

### String operations


```sql
SELECT
  CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
FROM
  `bigquery-samples.weather_geo.gsod`
WHERE
  station_number = 725030
  AND total_precipitation > 0
```


###  Join on Date


```sql
SELECT
  f.airline,
  SUM(IF(f.arrival_delay > 0, 1, 0)) AS num_delayed,
  COUNT(f.arrival_delay) AS total_flights
FROM
  `bigquery-samples.airline_ontime_data.flights` AS f
JOIN (
  SELECT
    CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
  FROM
    `bigquery-samples.weather_geo.gsod`
  WHERE
    station_number = 725030
    AND total_precipitation > 0) AS w
ON
  w.rainyday = f.date
WHERE f.arrival_airport = 'LGA'
GROUP BY f.airline
```

### Subquery

```sql
SELECT
  airline,
  num_delayed,
  total_flights,
  num_delayed / total_flights AS frac_delayed
FROM (
SELECT
  f.airline AS airline,
  SUM(IF(f.arrival_delay > 0, 1, 0)) AS num_delayed,
  COUNT(f.arrival_delay) AS total_flights
FROM
  `bigquery-samples.airline_ontime_data.flights` AS f
JOIN (
  SELECT
    CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
  FROM
    `bigquery-samples.weather_geo.gsod`
  WHERE
    station_number = 725030
    AND total_precipitation > 0) AS w
ON
  w.rainyday = f.date
WHERE f.arrival_airport = 'LGA'
GROUP BY f.airline
  )
ORDER BY
  frac_delayed ASC
```




























## Architecting a BigQuery project


[Building a Mobile Gaming Analytics Platform - a Reference Architecture](https://cloud.google.com/solutions/mobile/mobile-gaming-analysis-telemetry)


```shell


```

```shell


```

```shell


```



## More links

* https://github.com/GoogleCloudPlatform/python-docs-samples


