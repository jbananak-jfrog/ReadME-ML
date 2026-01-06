---
title: Use Cases
deprecated: false
hidden: false
metadata:
  title: Use-cases
  description: >-
    This featureset is engineered to conduct comprehensive data analysis by
    fetching data from the entire data source up to the last ingestion window
    during each ingestion job. In this process, all available data from the
    source is consumed up to the scheduled batch time. Essentially, the defined
    query or data retrieval process is executed against the specified data
    source (e.g., Snowflake), retrieving records based on conditions defined by
    the timestamp column until reaching the scheduled batch time.
  legacyUUIDs:
    - UUID-403f1168-3048-8445-c98d-a6a4881a6601
    - UUID-6f4e40f8-908e-6ea8-7bd6-8968b9632d9b
  robots: index
---
## Complete Snapshot of the Data Source

### Objective

This feature set is engineered to conduct comprehensive data analysis by fetching data from the entire data source up to the last ingestion window during each ingestion job. In this process, all available data from the source is consumed up to the scheduled batch time. Essentially, the defined query or data retrieval process is executed against the specified data source (for example, Snowflake), retrieving records based on conditions defined by the timestamp column until reaching the scheduled batch time.

This example, looks at joining a dimension table with another data source. The Full Read policy ensures that all relevant records are considered.

### Definition

This feature set uses two data sources as input - a dimension table and a fact table - both will be set to Full Read.

In addition, given that there are multiple data sources, a `timestamp_column_name` must be provided and exist in all data sources.

It’s important to note that the Feature Store always progresses with time, meaning that even though the entire data is read (input), the transformation result must progress in time.

For this purpose, two variables are exposed - `qwak_ingestion_start_timestamp` and `qwak_ingestion_end_timestamp`.

```python
@batch.feature_set(
    name = "full_read_featureset",
    key = "account_id",
    data_sources = {
        "account_dimension": ReadPolicy.FullRead,
        "account_fact": ReadPolicy.FullRead
    },
    timestamp_column_name = "process_time"
)
@batch.scheduling(cron_expression = "0 0 * * *")
@batch.execution_specification(cluster_template = ClusterTemplate.MEDIUM)
def transform():
    return SparkSqlTransformation(sql="""
        SELECT account_dimension.account_id,
                          account_fact.is_eligible,
               to_timestamp(${qwak_ingestion_start_timestamp}) AS processing_start,
               to_timestamp(${qwak_ingestion_end_timestamp}) AS process_time
        FROM account_dimension
             LEFT JOIN account_fact 
             ON account_dimension.account_id = account_fact.account_id"""
        )
```

## Timeframe-Based Data Retrieval

### Objective

The purpose of this feature set is to analyze data within a designated time period, utilizing the TimeFrame read policy akin to a sliding window mechanism. This policy ensures that only the latest data additions are retrieved, while keys or entities with no data within the specified timeframe are returned as null values. Specifically, the system focuses on data accumulated over the past 365 days.

### Definition

#### Data Source

The process begins by establishing a data source with a query designed to filter and compute data before the feature set is defined. When this data source is utilized, the query executes within the source itself before fetching the data. This approach facilitates the incorporation of feature store-specific logic that can be shared across multiple feature sets.

```
from frogml.feature_store.sources.data_sources import AthenaSource

athena_source = AthenaSource(
    name='my_athena_source',
    description='Athena data source for timeframe example',
    date_created_column='date_created',
    aws_region='region',
    s3_output_location='s3://path',
    workgroup='workgroup',
    query='''
SELECT
 users.id as user_id,
 readings.date_created,
 readings.reading
FROM users
JOIN readings
ON users.id = readings.user_id
ORDER BY readings.date_created
LIMIT 1000;
''',
)
```

#### Feature Set

This feature set uses the data source defined above, and uses the TimeFrame Read Policy.

```python
@batch.feature_set(
    name = "time_frame_featureset",
    key = "user_id",
    data_sources = {
        "my_athena_source": ReadPolicy.TimeFrame(
            days = 365,
        )},
    timestamp_column_name = "date_created"
)
@batch.scheduling(cron_expression = "0 0 * * *")
@batch.execution_specification(cluster_template = ClusterTemplate.LARGE)
def transform():
    return SparkSqlTransformation(sql="""
        SELECT user_id,
                          reading,
               date_created,
        FROM monitor_results_athena_source
             """
        )
```
