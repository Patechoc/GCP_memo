# Memo for Google BigQuery

Useful commands/scripts for use on Google Cloud Platform.

* [Definition](#definition)
* [Start web console for BigQuery](#start-web-console-for-bigquery)
* [Query large datasets in seconds (Examples)](#query-large-datasets-in-seconds-examples)
   * [Simple Query](#simple-query)
   * [Aggregate and Boolean functions](#aggregate-and-boolean-functions)
   * [String operations](#string-operations)
   * [Join on Date](#join-on-date)
   * [Subquery](#subquery)
* [Load and Export Data](#load-and-export-data)
   * [Load data from a CSV file](#load-data-from-a-csv-file)
   * [Load data from a JSON file](#load-data-from-a-json-file)
   * [Export BigQuery table in a bucket using the web UI (to .csv, .json or .avro format)](#export-bigquery-table-in-a-bucket-using-the-web-ui-to-csv-json-or-avro-format)
* [Architecting a BigQuery project](#architecting-a-bigquery-project)
* [More links](#more-links)



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

## Load and Export Data


### Load data from a CSV file

1. From the web GUI

* Go to https://bigquery.cloud.google.com
* You will need to use/create a dataset (within a given project) where the data will be stored.
* click on the `+` aside your dataset and follow the instructions (Table name in CAPITAL by convention)

2. From the command line tool: `bq`

* open `Cloud Shell` from the [Google Cloud Console](https://console.cloud.google.com) which already has `bq` installed
* download or Create a schema for your table

You can check that file online: https://storage.googleapis.com/cloud-training/CBP200/BQ/lab4/schema_flight_performance.json

```shell
 cat schema_flight_performance.json 
[
    {
        "name": "YEAR",
        "type": "INTEGER",
        "mode": "REQUIRED"
    },
    {
        "name": "QUARTER",
        "type": "INTEGER",
        "mode": "REQUIRED"
    },
    {
        "name": "MONTH",
        "type": "INTEGER",
        "mode": "REQUIRED"
    },
    {
        "name": "DAY_OF_MONTH",
        "type": "INTEGER"
    },
    {
        "name": "FULL_DATE",
        "type": "STRING"
    },
...
    {
        "name": "SECURITY_DELAY",
        "type": "INTEGER"
    },
    {
        "name": "LATE_AIRCRAFT_DELAY",
        "type": "INTEGER"
    }
]%                                                
```

Type the following command to download schema_flight_performance.json (the schema file for the table in this example) to your working directory.

```shell
curl https://storage.googleapis.com/cloud-training/CPB200/BQ/lab4/schema_flight_performance.json -o schema_flight_performance.json
```
The JSON files containing the data for your table are now stored in a Google Cloud Storage bucket.  They have URIs like the following: `gs://cloud-training/CPB200/BQ/lab4/domestic_2014_flights_*.json`


* use `bq` and type the following command to create a table named **flights_2014** in the **cpb101_flight_data** dataset, using data from files in Google Cloud Storage and the schema file stored on your virtual machine.

Note that your Project ID is stored as a variable in Cloud Shell (`$DEVSHELL_PROJECT_ID`) so there's no need for you to remember it. If you require it, you can view your Project ID in the command line to the right of your username (after the @ symbol).

```shell
bq load --source_format=NEWLINE_DELIMITED_JSON $DEVSHELL_PROJECT_ID:cpb101_flight_data.flights_2014 gs://cloud-training/CPB200/BQ/lab4/domestic_2014_flights_*.json ./schema_flight_performance.json
```

> Note
> 
> There are multiple JSON files in the bucket named according to the convention: domestic_2014_flights_*.json. The wildcard (*) character is used to include all of the .json files in the bucket.

Once the table is created, type the following command to verify table **flights_2014** exists in dataset **cpb101_flight_data**.

```shell
bq ls $DEVSHELL_PROJECT_ID:cpb101_flight_data
```

The output should look like the following:

```shell
pme@cloudshell:~ (southern-flash-208711)$ bq ls $DEVSHELL_PROJECT_ID:cpb101_flight_data
    tableId      Type    Labels   Time Partitioning
 -------------- ------- -------- -------------------
  AIRPORTS       TABLE
  flights_2014   TABLE
```

### Export BigQuery table in a bucket using the web UI (to .csv, .json or .avro format)

1. If you don't already have a bucket on Cloud Storage, create one from the [Storage section of the GCP console](http://console.cloud.google.com/storage). Bucket names have to be globally unique. (e.g. `demo-export-table-bigquery`)
2. Back to the [Google Cloud Console](http://console.cloud.google.com/) (in the incognito window) and using the menu, navigate into BigQuery web UI
3. Select the `AIRPORTS` table that you created recently, and using the "down" button to its right, select the option for **Export Table**.
4. In the dialog, specify `gs://<your-bucket-name>/bq/airports.csv` and click OK. (for me gs://demo-export-table-bigquery/bq/airports.csv)
5. Browse to your bucket and ensure that the .csv file has been created.
6. Test using a query:

```sql
SELECT *
FROM `southern-flash-208711.cpb101_flight_data.AIRPORTS`
```


## Advanced Capabilities in BigQuery

### WITH, COUNT(DISTINCT)

```sql
WITH WashinghtonStations AS (
SELECT ...
...
)
SELECT washinghton_stations.name, ...
FROM WashinghtonStations AS washinghton_stations
ORDER BY rainy_days DESC;
```


### Processing BigQuery Data Types (ARRAY/STRUCT, JOIN condition, ...)

* `EXTRACT` to extract a date from a timestamp.

```sql
SELECT
  author.email,
  diff.new_path AS path,
  author.date
FROM
  `bigquery-public-data.github_repos.commits`,
  UNNEST(difference) diff
WHERE
  EXTRACT(YEAR
  FROM
    author.date)=2016
LIMIT 10
```

* using `ARRAY` and `STRUCT` to build for exmaple a table with several entries for a given date.

```sql

```

* `STARTS_WITH` to find words that start with a prefix

```sql

```


* `JOIN` condition example

```sql

```

### Standard SQL functions

https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators

* Aggregate functions
* String functions (e.g. `REGEXP_CONTAINS`)
* Analytic (window) functions
  * Standard aggregations (SUM, AVG, MIN, MAX, COUNT, ...)
  * Navigation functions (LEAD, LAG, NTH_VALUE, ...)
  * Ranking and numbering functions (CUME_DIST, DENSE_RANK, ROW_NUMBER, RANK, PERCENT_RANK, ...)
* Datetime functions
* Array functions
* Other functions and operators

```sql
WITH
  commits AS (
  SELECT
    author.email,
    EXTRACT(DAYOFWEEK
    FROM
      author.date) BETWEEN 2
    AND 6 is_weekday,
    LOWER(REGEXP_EXTRACT(diff.new_path, r'\.([^\./\(~_ \- #]*)$')) lang,
    diff.new_path AS path,
    author.date
  FROM
    `bigquery-public-data.github_repos.commits`,
    UNNEST(difference) diff
  WHERE
    EXTRACT(YEAR
    FROM
      author.date)=2016)
SELECT
  lang,
  is_weekday,
  COUNT(path) AS numcommits
FROM
  commits
WHERE
  LENGTH(lang) < 8
  AND lang IS NOT NULL
  AND REGEXP_CONTAINS(lang, '[a-zA-Z]')
GROUP BY
  lang,
  is_weekday
HAVING
  numcommits > 100
ORDER BY
  numcommits DESC
```

ref. [The top weekend programming languages — based on GitHub’s activity](https://medium.com/@hoffa/the-top-weekend-languages-according-to-githubs-code-6022ea2e33e8)

### Date and time functions

Multiple ways to create a date:

* `DATE(year, month, day)`
* `DATE(timestamp)` and supports time zone
* `DATETIME(date, time)` using DATE and TIME objects

### User-defined functions (`CREATE TEMPORARY FUNCTION`)

written in **SQL** or **Javascript**, they are temporary. But user-defined functions have constraints.

* programming logic
* loops, complex conditions non-trivial string parsing (not supported by standard SQL)

```sql

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
* [How to query public data sets using BigQuery (e.g. Github data)](https://cloud.google.com/bigquery/public-data/github)


