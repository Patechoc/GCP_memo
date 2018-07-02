# Memo for Google BigQuery

Useful commands/scripts for use on Google Cloud Platform.



## Definition

BigQuery is the PetaBytes-scale datawarehouse on GCP (for <u>**immutable**</u> **very-large** datasets)



## Start web console for BigQuery

Use one of these:

* https://bigquery.cloud.google.com
* https://console.cloud.google.com/ then from the 'hamburger' menu, look for `BigQuery` under  the `Big Data` section.


## Query large datasets in seconds


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

## Architecting a BigQuery project


[Building a Mobile Gaming Analytics Platform - a Reference Architecture](https://cloud.google.com/solutions/mobile/mobile-gaming-analysis-telemetry)




```shell


```

```shell


```

```shell


```

```shell


```



## More links

* https://github.com/GoogleCloudPlatform/python-docs-samples


