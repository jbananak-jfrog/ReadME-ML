---
title: Streaming Feature Sets
deprecated: false
hidden: false
metadata:
  title: Streaming Feature Set
  description: >-
    A Streaming Feature Set is identical to a Batch Feature Set in terms of its
    use (retrieving online/offline features), but instead of reading data from a
    Batch Source, it reads from an infinite Stream Source - For example, Apache
    Kafka.
  legacyUUIDs:
    - UUID-51a4a590-394b-678b-cbe1-6ddb634d8c63
    - UUID-21752cae-37fa-d69c-fbd0-e845653a3f7f
  robots: index
---
A Streaming Feature Set is identical to a Batch Feature Set in terms of its use (retrieving online/offline features), but instead of reading data from a Batch Source, it reads from an infinite Stream Source - For example, Apache Kafka.

The 2 basic building blocks that define a Streaming Feature Set are its [Streaming Source](/docs/data-sources#streaming-data-sources) (for example, Kafka) and a Transformation.

See the Streaming Sources section for more details regarding the available Streaming Sources.

<Callout icon="❗️" theme="error">
  **Important** - **_Python Version_**

  Please note that Python 3.8 is required for all Streaming capabilities.
</Callout>

## Streaming Feature Set Creation

Creating a streaming feature set involves defining a [feature transformation](/docs/streaming-feature-set#transformations) function and utilizing the `@streaming.feature_set` decorator along with the specified parameters:

▶ **To create a streaming feature set:**

1. **Feature Transformation Function:**

   * Begin by crafting a feature transformation function tailored to the desired processing of your raw data.
2. **Decorator Implementation:**

   * Apply the `@streaming.feature_set` decorator to your transformation function, ensuring to include the following parameters:

     * `name`: If not explicitly defined, the decorated function's name is used. The name field is restricted to **alphanumeric** and **hyphen** characters, with a maximum length of 40 characters.
     * `key`: Specify the key for which to calculate the features in the feature set.
     * `data_sources`: Provide a list containing the names of relevant [streaming data sources](/docs/data-sources#streaming-data-sources) that the feature set data will be ingested from. **Currently streaming feature sets support only a single data source configuration.**
     * `timestamp_column_name`:The name of the column in the data source that contains timestamp information. This is used to order the data chronologically and ensure that the feature values are updated in the correct order.
     * `offline_scheduling_policy`: A crontab definition of the the offline ingestion policy - which affects the data freshness of the offline store. defaults to `*/30 * * * *` (every 30 minutes)
     * `online_trigger_interval`: Defines the online ingestion policy - which affects the data freshness of the online store. Defaults to 5 seconds.

These steps ensure the seamless creation of a batch feature set, enabling you to define the transformation logic and specify the essential parameters for efficient feature extraction and processing within the JFrog ML ecosystem.

### Streaming Feature Set Example

This example:

* Creates a streaming feature set, with online store freshness of 30 seconds and an hourly offline store freshness
* Ingests data from the `my_kafka_source` source.
* Creates a transformed feature vector with the fields: `user_id`,

  `registration_country` and `registration_device`
* Ingests the feature vector into the <Anchor label="Frog ML Feature Store" target="_blank" href="https://jfrog.com/blog/what-is-a-feature-store-in-ml-and-do-i-need-one/">Frog ML Feature Store</Anchor>

```python
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.transformations import SparkSqlTransformation

@streaming.feature_set(
    name="user-features",
    key="user_id",
    data_sources=["my_kafka_source"],
    timestamp_column_name="date_created",
    offline_scheduling_policy="0 * * * *",
    online_trigger_interval=30
)
def user_features():
    return SparkSqlTransformation(sql="""
        SELECT user_id,
               registration_country,
               registration_device,
               date_created
        FROM my_kafka_source""")
```

### Adding Metadata

An optional decorator for defining feature set metadata information of:

`owner` - User name of feature set owner

`description` - Describe what does this feature set do

`display_name` - Alternative feature set name for UI display

```python
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.transformations import SparkSqlTransformation

@streaming.feature_set(
    name="user-features",
    key="user_id",
    data_sources=["my_kafka_source"],
    timestamp_column_name="date_created",
    offline_scheduling_policy="0 * * * *",
    online_trigger_interval=30
)
@streaming.metadata(
    owner="John Doe",
    display_name="User Aggregation Data",
    description="User origin country and devices"
)
def user_features():
    return SparkSqlTransformation(sql="""
        SELECT user_id,
               registration_country,
               registration_device,
               date_created
        FROM my_kafka_source""")
```

## Specifying Execution Resources

In JFrog ML, the allocation of resources is crucial for streaming execution jobs, often termed as the `cluster template`. This template determines resources like CPU, memory, and temporary storage - all essential for executing user-defined transformations and facilitating feature ingestion into designated stores.

<Callout icon="📘" theme="info">
  _**Cluster Template**_

  The default size for the cluster template is `MEDIUM` if none is explicitly specified.
</Callout>

For streaming feature sets, two different resource specifications are provided:

* `online_cluster_template`**online** feature store ingestion job resources
* `offline_cluster_template`**offline** feature store ingestion job resources

```python
# Python
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.execution_spec import ClusterTemplate
from frogml.core.feature_store.feature_sets.transformations import SparkSqlTransformation


@streaming.feature_set(
    name="user-features",
    key="user_id",
    data_sources=["my_kafka_source"],
    timestamp_column_name="date_created",
    offline_scheduling_policy="0 * * * *",
    online_trigger_interval=30
)
@streaming.execution_specification(
    online_cluster_template=ClusterTemplate.SMALL,
    offline_cluster_template=ClusterTemplate.MEDIUM,
)
def user_features():
    return SparkSqlTransformation(sql="""
        SELECT user_id,
               registration_country,
               registration_device,
               date_created
        FROM my_kafka_source""")
```

## Transformations

Row-level transformations that are applied to the data (in a streaming fashion) - these transformations produce the actual features.

### SQL Transformations

Row-level arbitrary SQL, with support for PySpark Pandas UDFs, leverages vectorized computation using PyArrow.

```python
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.transformations import SparkSqlTransformation

@streaming.feature_set(
    name='sample_fs',
    key='user_id',
    data_sources=['sample-streaming-source'],
    timestamp_column_name='timestamp'
)
def transform():
    return SparkSqlTransformation(
        sql="select timestamp, user_id, first_name, last_name from sample_source"
    )
```

Or, when using a Pandas UDF:

```python
import pandas as pd
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.transformations import (
    Column,
    Schema,
    SparkSqlTransformation,
    Type,
    frogml_pandas_udf,
)


@frogml_pandas_udf(output_schema=Schema([Column(type=Type.long)]))
def plus_one(column_a: pd.Series) -> pd.Series:
    return column_a + 1


@frogml_pandas_udf(output_schema=Schema([Column(type=Type.long)]))
def mul_by_two(column_a: pd.Series) -> pd.Series:
    return column_a * 2


@streaming.feature_set(
    name="sample_fs",
    key="user_id",
    data_sources=["sample-streaming-source"],
    timestamp_column_name="timestamp",
)
def transform():
    return SparkSqlTransformation(
        sql="select timestamp, mul_by_two(value) as col1, plus_one(value) as col2 from ds1",
        functions=[plus_one, mul_by_two],
    )
```

### Full-DataFrame Pandas UDF Transforms

Just like the Pandas UDFs supported in SQL Transforms, but defined on the entire DataFrame (no need to write any SQL):

```python
import pandas as pd
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.transformations import (
    Column,
    Schema,
    Type,
    UdfTransformation,
    frogml_pandas_udf,
)


@frogml_pandas_udf(
    output_schema=Schema(
        [
            Column(name="rate_mul", type=Type.long),
            Column(name="date_created", type=Type.timestamp),
        ]
    )
)
def func(df: pd.DataFrame) -> pd.DataFrame:
    df = pd.DataFrame(
        {"rate_mul": df["value"] * 1000, "date_created": df["date_created"]}
    )
    return df


@streaming.feature_set(
    name="user-features",
    key="user_id",
    data_sources=["my_kafka_source"],
    timestamp_column_name="date_created",
    offline_scheduling_policy="0 * * * *",
    online_trigger_interval=30,
)
def user_features():
    return UdfTransformation(function=func)
```

#### Event-time Aggregations

In addition to row-level operations, event-time aggregations are also supported, with **EXACTLY ONCE** semantics.

The system employs a highly optimized proprietary implementation, partially based on open source Apache Spark ™. This implementation is designed to optimize for data freshness, low serving latency, and high throughput. It handles multiple overlapping time windows (long and short) and out-of-order data (late arrivals) without the intense resource consumption often incurred in these cases.

Enable aggregations by adding them on top of the row-level transform. For example:

```python
from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.transformations import (
    FrogmlAggregation,
    SparkSqlTransformation,
)


@streaming.feature_set(
    key="user_id",
    data_sources=["transaction_stream"],
    timestamp_column_name="timestamp",
    name="my_streaming_agg_fs",
)
def transform():
    sql = "SELECT timestamp, user_id, transaction_amount, is_remote, offset, topic, partition FROM transaction_stream"

    return (
        SparkSqlTransformation(sql)
        .aggregate(FrogmlAggregation.avg("transaction_amount"))
        .aggregate(FrogmlAggregation.boolean_or("is_remote"))
        .aggregate(FrogmlAggregation.sum("transaction_amount"))
        .by_windows("1 minute", "1 hour", "3 hour", "1 day", "7 day")
    )
```

The example above configures calculations for the average transaction amount, sum of transaction amounts, and a boolean check for remote transactions. These metrics are computed across 5 time windows, ranging from 1 minute to 7 days.

**Configuration breakdown:**

1. **Row-level transform:** Define the regular row-level transform for streaming - using either an SQL transform (with or without pandas UDFs) or a full-dataframe pandas udf.
   * **Note:** All aggregations target the output columns of this transform. Perform any necessary row-level modifications (string manipulation, currency conversion, boolean conditions, etc.) prior to aggregation.\
   * **Required Metadata:** Currently, three Kafka metadata columns (`offset`, `topic`,  and `partition`) must be selected. These are used internally by JFrog ML to guarantee compliance with **EXACTLY ONCE** semantics.
2. **Declarative aggregates:** Add aggregations. sequentially in a chained fashion; the above example utilizes `avg`, `sum`, and `boolean_or`.
3. **Time windows:** Define the time windows for aggregation. The combination of aggregations and windows determines the total feature count. In this example, 3 aggregates multiplied by 5 time windows results in a `Featureset` containing 15 features.

JFrog currently supports the following aggregates:

* **SUM** - a sum of column, for example, `FrogmlAggregation.sum("transaction_amount")`
* **COUNT** - count (not distinct), a column is specified for API uniformity. for example, `FrogmlAggregation.count("transaction_amount")`
* **AVERAGE** - mean value, for example `FrogmlAggregation.avg("transaction_amount")`
* **MIN** - minimum value, for example `FrogmlAggregation.min("transaction_amount")`
* **MAX** - maximum value, for example `FrogmlAggregation.max("transaction_amount")`
* **BOOLEAN OR** - boolean or, defined over a boolean column, for example `FrogmlAggregation.boolean_or("is_remote")`
* **BOOLEAN AND** - boolean and, defined over a boolean column, for example `FrogmlAggregation.boolean_and("is_remote")`
* **Sample Variance** - `FrogmlAggregation.sample_variance("transaction_amount")`
* **Sample STDEV** - `FrogmlAggregation.sample_stdev("transaction_amount")`
* **Population Variance** - `FrogmlAggregation.population_variance("transaction_amount")`
* **Population STDEV** - `FrogmlAggregation.population_stdev("transaction_amount")`

In addition, it's also possible to add an Alias - a prefix for the result feature name.

By default, an aggregate results in a feature named `<aggregate_name>_<column_name>_<window_size>`, for each window defined.

In some cases, it may be better to a have different prefix rather than `<aggregate_name>_<column_name>`- in these cases, specify an alias. For example:

```
SparkSqlTransformation(sql)\
    .aggregate(FrogmlAggregation.avg("transaction_amount"))\
    .aggregate(FrogmlAggregation.boolean_or("is_remote").alias("had_remote_transaction"))\
    .by_windows("1 minute, 1 hour")
```

In the above sample, the `boolean_or` is aggregated, so it's now called `had_remote_transactions_<window>`, where `<window>` is the time window.  It will result in 4 features: `avg_transaction_amount_1m`, `avg_transaction_amount_1h`, `had_remote_transactions_1m`, `had_remote_transactions_1h`

## Event-time Aggregations Backfill

For streaming aggregation feature sets, adding backfill spec will populate historical features values from _batch_ data sources before deploying the actual streaming `featureset`

The Streaming Backfill parameters are:

* **start_datetime**: Date & time from which to start fetching values.
* **end_datetime**: Date & time from which to end fetching values.

<Callout icon="❗️" theme="error">
  **Important**

  **end_datetime** must be divisible by slice size/
</Callout>

* **transform**: An SQL transformation that selects the relevant features from the batch sources. **Note:** it's output schema must include the **raw** streaming source schema.
* **data_source_specs**: List of existing batch data source names from whichto fetch.
* **execution_spec**: [Optional] resource template for backfill step.

```python
from datetime import datetime

from frogml.feature_store.feature_sets import streaming
from frogml.core.feature_store.feature_sets.execution_spec import ClusterTemplate
from frogml.feature_store.feature_sets.streaming_backfill import (
    BackfillBatchDataSourceSpec,
)
from frogml.core.feature_store.feature_sets.transformations import (
    FrogmlAggregation,
    SparkSqlTransformation,
)


@streaming.feature_set(
    key="user_id",
    data_sources=["users_registration_stream"],
    timestamp_column_name="reg_date",
    name="my_backfilled_streaming_agg_fs",
)
@streaming.backfill(
    start_date=datetime(2022, 1, 1, 0, 0, 0),
    end_date=datetime(2023, 9, 1, 0, 0, 0),
    data_sources=[
        BackfillBatchDataSourceSpec(
            data_source_name="batch_backfill_source_name",
            start_datetime=datetime(2023, 1, 1, 0, 0, 0),
            end_datetime=datetime(2023, 8, 1, 0, 0, 0),
        )
    ],
    backfill_cluster_template=ClusterTemplate.XLARGE,
    backfill_transformation=SparkSqlTransformation(
        "SELECT user_id, amount, reg_date FROM backfill_data_source"
    ),
)
def user_streaming_features():
    return (
        SparkSqlTransformation("SELECT user_id, amount, reg_date FROM data_source")
        .aggregate(FrogmlAggregation.avg("amount"))
        .by_windows("1 minute, 1 hour")
    )
```

The example above extracts the `user_id` and `amount` features from `batch_backfill_source_name` (which is an existing batch data source), and renames them to match the desired feature name of the **raw** stream source column names.

The data is between 1/1/2020 and 1/9/2022.

If it's required to specify a specific datetime filter for each batch source (meaning selecting from different sub start and end times for each source), `BackfillBatchDataSourceSpec` needs to be passed.

<Callout icon="📘" theme="info">
  Filtering per data source is optional, but keep in mind that the general start and end time filter set for the backfill will be lower and upper limits for any sub specific backfill source filter
</Callout>

```python
data_source_specs = [
    BackfillBatchDataSourceSpec(
        data_source_name="sample-batch-source",
        start_datetime=datetime(2021, 1, 1),
        end_datetime=datetime(2021, 3, 1),
    )
]
```

### Specifying Auxiliary Sinks

<Callout icon="📘" theme="info">
  Auxiliary sinks are available for streaming feature sets without any aggregations
</Callout>

Remember that Feature sets ingest data from data sources and produce features that are then stored in the online store and the offline store. but what happens if we'd like the features to also be sent to a destination of our choice? That's where auxiliary sinks come in.

An auxiliary sink is simply another destination for computed feature values. For example, you may want the feature values to also be published into a Kafka Topic from which you can consume them for various purposes.

Another strong use case for auxiliary sinks, is for creating dependencies between streaming feature sets. If you have a feature set X and you'd like to have another feature set Y that consumes whatever X is producing, you can configure X with an auxiliary sink (for example - a Kafka sink), then use the sink (i.e., topic) as a data source for feature set Y.

### Attachment Points

Remember that a streaming feature set ingests data using 2 separate Spark clusters - one ingests into the online store (constantly running) while the other periodically ingests into the offline store. This architecture maximizes the data freshness in the online store,  while simultaneously controlling the cost and ensuring consistency, preventing training-serving skew.

Eventually, all data is ingested in exactly the same way into both online and offline - the only difference is that the ingestion into the online store is continuous while ingestion into the offline store is periodic.

When defining auxiliary sinks, select the **attachment point** - meaning when the features will be written to the sink:

* If selecting an **online attachment point**, the features are written into the sink when they are written into the online store - meaning that the sink will have high data freshness, but will add an overhead to the online ingestion program, possibly lowering its data freshness.
* Conversely, if selecting an **offline attachment point**, the features are written to the sink whenever they are written to the offline store. This ensures that the data freshness in the online store remains unchanged, but yields a lower data freshness for the sink itself.

<Callout icon="📘" theme="info">
  Auxiliary sinks are guaranteed **At-Least-Once** semantics.
</Callout>

### Auxiliary Sink Types

#### Kafka Sinks

Example showing how two different auxiliary sinks are created:

* The first one uses a kafka topic called "online_topic" and uses an Online Streaming Attachment Point.
* The second one uses another topic and uses an Offline Streaming Attachment Point.

```python
# python
from frogml.feature_store.data_sources import SslAuthentication, SaslAuthentication, SaslMechanism, \
    SecurityProtocol
from frogml.feature_store.feature_sets import streaming

from frogml.core.feature_store.feature_sets.transformations.transformations import (
    SparkSqlTransformation,
)
from frogml.feature_store.sinks.kafka import KafkaSink, MessageFormat
from frogml.core.feature_store.sinks.streaming.attachment import OnlineStreamingAttachmentPoint, \
    OfflineStreamingAttachmentPoint

# Authenticate using SSL
online_sink: KafkaSink = KafkaSink(
    name="my_online_sink",
    topic="online_topic",
    bootstrap_servers="bootstrap.server1.com:9022",
    message_format=MessageFormat.JSON,
    auth_configuration=SslAuthentication(),
    attachment_point=OnlineStreamingAttachmentPoint(),
)

# Authenticate using SASL
offline_sink: KafkaSink = KafkaSink(
    name="my_offline_sink",
    topic="offline_topic",
    bootstrap_servers="bootstrap.server2.com:9022,bootstrap.server3.com:9022",
    message_format=MessageFormat.JSON,
    auth_configuration=SaslAuthentication(username_secret="username-secret-name",
                                          password_secret="password-secret-name",
                                          sasl_mechanism=SaslMechanism.SCRAMSHA256,
                                          security_protocol=SecurityProtocol.SASL_SSL),
    attachment_point=OfflineStreamingAttachmentPoint(),
)


@streaming.feature_set(
    key="user_id",
    data_sources=["user_updates"],
    timestamp_column_name="reg_date",
    auxiliary_sinks=[online_sink, offline_sink],
)
def user_streaming_features():
    return (
        SparkSqlTransformation(
            "SELECT user_id,"
            " reg_country,"
            " reg_date,"
            " email,"
            " address"
            " FROM data_source"
        )
    )
```

#### Output Formats for Kafka Auxiliary Sinks

##### JSON

When selecting JSON, the features are written into the the topic according to the following format:

* **Message Key:** the key of the feature vector (for example, the value of "user_id" in the above example).
* **Message Value:** a JSON string according to the following specification:

```json
{
  "featureset_name": "<featureset_name>",
  "featureset_id": "<featureset_id>",
  "timestamp": "<timestamp>", # will always be called "timestamp", regardless of the actual name fo the timestamp column
  "key": {
    # all columns comprising the key, e.g. "user_id": "abcd"
  },
  "features": {
    # all features together with their value, for example:
    "reg_country": "US",
    "email": "user@company.com",   
    "address": "Planet Earth"
  }
}
```

<br />
