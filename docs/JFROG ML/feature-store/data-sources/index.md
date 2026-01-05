---
title: Data Sources
deprecated: false
hidden: false
metadata:
  title: Data Sources
  description: >-
    JFrog ML data sources are used to configure connections to your data. Data
    sources are used in order to create feature sets.
  legacyUUIDs:
    - UUID-b1e8dbca-38d1-8de3-592d-03bc66a9b81c
    - UUID-b1e28037-f8c2-43b6-79bc-53a94599b691
    - UUID-f911515c-6ed1-5336-4ae4-0e23eab4e366
    - UUID-0124c656-b550-2d83-b4a7-bb3bae2ca04c
    - UUID-ffb348a1-3557-cdb5-5e19-7ca647688aa0
    - UUID-766c9d8f-defa-5fcc-8488-0e14847ae833
  robots: index
---
JFrog ML data sources are used to configure connections to your data. Data sources are used in order to create feature sets.

There are two main types of data sources:

<Cards>
  <Card title="Batch" href="/docs/data-sources#batch-data-sources">
    Data-at-rest sources of data, such as Athena, Snowflake, and Redshift.
  </Card>

  <Card title="Streaming" href="/docs/data-sources#streaming-data-sources">
    Data in motion sources, such as Kafka and Kinesis.
  </Card>
</Cards>

* [**Batch**](/docs/data-sources#batch-data-sources): [Data-at-rest sources of data, such as Athena, Snowflake, and Redshift](/docs/data-sources#batch-data-sources)
* **[Streaming](/docs/data-sources#streaming-data-sources)**: Data in motion sources, such as Kafka and Kinesis.

▶ **To connect to a data source:**

1. Enable network connectivity between the data sources and JFrog ML cluster if they are not publicly accessible.
2. Grant JFrog ML access to your data lake components by creating read-only service accounts and/or IAM roles.

## Defining Data Sources

Data sources can be defined and registered programmatically via JFrog ML SDK and CLI, or created altogether via the JFrog ML UI.

### Via JFrogML SDK/CLI

JFrog ML provides Python classes to define any data source type using the `frogml.feature_store.data_sources` package.

For example, you can define a CsvSource to read from an S3 based CSV file as follows:

```
from frogml.feature_store.data_sources import CsvSource

# The S3 anonymous config class is required for public S3 buckets
from frogml.feature_store.data_sources import AnonymousS3Configuration

# Create a CsvSource object to represent a CSV data source 
# This example uses a CSV file from a public S3 bucket

csv_source = CsvSource(
    name='credit_risk_data',                                    # Name of the data source
    description='A dataset of personal credit details',         # Description of the data source
    date_created_column='date_created',                         # Column name of the column that holds the creation date
    path='s3://qwak-public/example_data/data_credit_risk.csv',  # S3 path to the CSV file 
    filesystem_configuration=AnonymousS3Configuration(),        # Configuration for anonymous access to S3
    quote_character='"',                                        # Character used for quoting in the CSV file
    escape_character='"'                                        # Character used for escaping in the CSV file
)
```

<Callout icon="📘" theme="info">
  You must run the `frogml features register` command for that object to register **Data Sources** defined with the FrogML SDK in the cloud platform.
</Callout>

### Via the UI:

1. From the JFrog Platform menu, select **AI/ML** > **Data Sources**.
2. Click **Create new data source**.
   <Callout icon="📘" theme="info">
     You must have a data source group set up before you can set up a data source.
   </Callout>
3. Select the required data source type from the list.
4. Fill in the form (mandatory fields are marked with an asterisk).
5. Test the connection to the data source to verify it is operating (Click **Test connection**).
6. Click **Save**. The data source is created.

## Registering Data Sources

To register a data source class defined with the SDK you can use the JFrog ML CLI `features` command as follows:

```
frogml features register -p data_source.py
```

## Deleting Data Sources

To delete a data source, execute the following `frogml` command in the terminal:

```
frogml features delete --data-source <data-source-name>
```

<Callout icon="⚠️" theme="warning">
  **Warning** - _**Deleting Data Sources In Use**_

  Before you can delete a data source that is linked to one or more Feature Sets, you must either remove those Feature Sets or reassign them to a different data source.
</Callout>

## Batch Data Sources

Batch data sources enable you to configure connections to **data-at-rest** sources of data.

To define a batch data source, create a configuration object that connects to the raw data source.

Batch data sources share three common parameters:

| Parameter                | Description                                                                                                                                                                              |
| :----------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **name**:                | A unique data source identifier used to address it from a feature set object, may contain only characters, numbers and _.                                                                |
| **description**:         | A general description.                                                                                                                                                                   |
| **date_created_column**: | Used to filter the data by the batch's start time/end time. date_created_column must be present in the database. This column must hold the timestamp which represents each records time. |

<Callout icon="⚠️" theme="warning">
  **Warning** - `date_created_column`:

  Values in this column must be increasing chronologically. If the date_created is prior to the previous date, the event will not be ingested. Missed data can be added using the <Anchor label="Backfill" title="Backfill" href="/docs/backfill">Backfill</Anchor>.
</Callout>

<Callout icon="📘" theme="info">
  **Note**

  Default timestamp format for `date_created_column` should be `yyyy-MM-dd'T'HH:mm:ss`, optionally with `[.SSS][XXX]`. For example: `2020-01-01T00:00:00`.
</Callout>

### Registering New Data Sources

When registering a batch data source, the JFrog ML System will try to validate it, meaning it will attempt to fetch a sample to verify that the system can query the data source.

Additionally, batch data sources support the following validation function:

```
def get_sample(self, number_of_rows: int = 10) -> DataFrame:
```

Usage example:

```
from frogml.feature_store.data_sources import ParquetSource, AnonymousS3Configuration

parquet_source = ParquetSource(
    name='parquet_source',
    description='a parquet source description',
    date_created_column='date_created',
    path="s3://bucket-name/data.parquet",
    filesystem_configuration=AnonymousS3Configuration()
)

pandas_df = parquet_source.get_sample()
```

When invoking this function, the FrogML System will validate the data source before returning a Pandas DataFrame, meaning that if an error occurred while trying to fetch a sample, the system indicates at which stage it failed.

For example, it can fail:

* When connecting to the specified bucket.
* When the date_created_column is not the right type or does not exist.

### Available Data Source Types

| [Snowflake](/docs/data-sources#snowflake) | [BigQuery](/docs/data-sources#bigquery)     | [MongoDB](/docs/data-sources#mongodb)             | [Amazon S3 Stored Files](/docs/data-sources#amazon-s3-stored-files) |
| :---------------------------------------- | :------------------------------------------ | :------------------------------------------------ | :------------------------------------------------------------------ |
| [Redshift](/docs/data-sources#redshift)   | [MySQL](/docs/data-sources#mysql)           | [Postgres](/docs/data-sources#postgres)           | [Clickhouse](/docs/data-sources#clickhouse)                         |
| [Vertica](/docs/data-sources#vertica)     | [AWS Athena](/docs/data-sources#aws-athena) | [Unity Catalog](/docs/data-sources#unity-catalog) |                                                                     |

#### Snowflake

In order to create a Snowflake connection, before creating a connector make sure you have the following:

1. Snowflake User configured to unencrypted <Anchor label="key-pair authentication" target="_blank" href="https://docs.snowflake.com/en/user-guide/key-pair-auth">key-pair authentication</Anchor> (Read-Only access required).
2. Connectivity between JFrog ML environment and Snowflake host.

   There are two distinct ways to use the Snowflake connector:

   1. Providing `table`.

      ```
      from frogml.feature_store.data_sources import SnowflakeSource

      snowflake_source = SnowflakeSource(
          name='snowflake_source',
          description='a snowflake source description',
          date_created_column='insert_date_column',
          host='<SnowflakeAddress/DNS:port>',
          username_secret_name='jfrogml_secret_snowflake_user', # use secret service
          pem_private_key_secret_name='jfrogml_secret_snowflake_pem_private_key', # use secret service
          database='db_name',
          schema='schema_name',
          warehouse='data_warehouse_name',
          table='snowflake_table'
      )
      ```
   2. Providing `query`.

      ```
      from frogml.feature_store.data_sources import SnowflakeSource

      snowflake_source = SnowflakeSource(
          name='snowflake_source',
          description='a snowflake source description',
          date_created_column='insert_date_column',
          host='<SnowflakeAddress/DNS:port>',
          username_secret_name='jfrogml_secret_snowflake_user', # use secret service
          pem_private_key_secret_name='jfrogml_secret_snowflake_pem_private_key', # use secret service
          database='db_name',
          schema='schema_name',
          warehouse='data_warehouse_name',
          query='select feature1, feature2 from snowflake_table'
      )
      ```

<Callout icon="📘" theme="info">
  **Note**

  JFrog ML only supports **unencrypted** private keys without the key delimiters (begin and end). See <Anchor label="key-pair authentication" target="_blank" href="https://docs.snowflake.com/en/user-guide/key-pair-auth">key-pair authentication</Anchor>.
</Callout>

#### BigQuery

To access a BigQuery source, please download the _credentials.json_ file from GCP to your the local file system.

##### Permissions

The following permissions **must be applied** to the provided credentials in the _credentials.json_ file.

```
bigquery.tables.create
bigquery.tables.getData  
bigquery.tables.get
bigquery.readsessions.* bigquery.jobs.create
```

##### Uploading Credentials

Once you've downloaded _credentials.json_, encode it with base64 and set it as a JFrog ML secret using the JFrog ML Secret Service.

```
import json
import base64
from frogml.core.clients.secret_service import SecretServiceClient

with open('/path/of/credentials/credentials.json', 'r') as f:
    creds = json.load(f)

creds64 = base64.b64encode(json.dumps(creds).encode('utf-8')).decode('utf-8')

secrets_service = SecretServiceClient()
secrets_service.set_secret(name='qwak_secret_big_query_creds', value=creds64)
```

##### Connecting to BigQuery

There are two distinct ways to use the BigQuery connector:

##### 1. Providing `dataset` and `table`

```
from frogml.feature_store.data_sources import BigQuerySource

some_bigquery_source = BigQuerySource(
    name='big_query_source',
    description='a bigquery source description',
    date_created_column='date_created',
    credentials_secret_name='qwak_secret_big_query_creds',
    dataset='dataset_name',
    table='table_name',
    project='project_id',
    materialization_project='materialization_project_name'
    parent_project='parent_project',
    views_enabled=False
)
```

##### 2. Providing `sql`

```
from frogml.feature_store.data_sources import BigQuerySource

big_query_source = BigquerySource(
    name='big_query',
    description='a big query source description',
    date_created_column='date_created',
    credentials_secret_name='bigquerycred',
    project='project_id',
    sql="""SELECT l.id as id, 
           SUM(l.feature1) as feature1, 
           SUM(r.feature2) as feature2,
           MAX(l.date_created) as date_created,
           FROM `project_id.dataset.left` AS l
           JOIN `project_id.dataset.right` as r
           ON r.id = l.id 
           GROUP BY id""",
    parent_project='',
    views_enabled=False
)
```

#### MongoDB

```
from frogml.feature_store.data_sources.batch.mongodb import MongoDbSource 

mongo_source = MongoDbSource(
    name='mongo_source',
    description='a mongo source description',
    date_created_column='insert_date_column',
    hosts='<MongoAddress/DNS:Port>',
    username_secret_name='qwak_secret_mongodb_user', #uses the Qwak Secret Service
    password_secret_name='qwak_secret_mongodb_pass', #uses the Qwak Secret Service
    database='db_name',
    collection='collection_name',
    connection_params='authSource=admin'
)
```

#### Amazon S3 Stored Files

##### Ingesting Data from Parquet Files

AWS S3 filesystem data sources support explicit credentials for a custom bucket (default: frogml bucket).

To access more of your data from a different S3 bucket, use this optional configuration.

Once creating the relevant secrets using the JFrog ML-CLI you can use:

```
from frogml.feature_store.data_sources import ParquetSource, AwsS3FileSystemConfiguration

parquet_source = ParquetSource(
    name='my_source',
    description='some s3 data source',
    date_created_column='DATE_CREATED',
    path='s3://mybucket/parquet_test_data.parquet',
    filesystem_configuration=AwsS3FileSystemConfiguration(
        access_key_secret_name='mybucket_access_key',
        secret_key_secret_name='mybucket_secret_key',
        bucket='mybucket'
    )
)
```

<Callout icon="❗️" theme="error">
  **Important** - _**Timestamp Column**_

  Ensure that the timestamp column in your Parquet file(s) is represented using the appropriate PyArrow timestamp data type with microsecond precision.

  You can achieve this by casting the timestamp column to the desired precision. Here's an example:

  ```
  timestamp_column_microseconds = timestamp_column.cast('timestamp[us]')
  ```

  In the above code snippet, `timestamp_column_microseconds` refers to the modified timestamp column with microsecond precision. This column represents information like the date and time that a record was created, denoted as `date_created`.

  Using Pandas timestamp data types, like `datetime[ns]` or `int64` will result in an error when fetching data from the Parquet source.
</Callout>

##### Ingesting Data from CSV Files

CSV access works like reading a Parquet file from S3. We either specify the AWS access keys as environment variables or access a public object.

```
from frogml.feature_store.data_sources import CsvSource, AnonymousS3Configuration

csv_source = CsvSource(
    name='csv_source',
    description='a csv source description',
    date_created_column='date_created',
    path="s3://bucket-name/data.csv",
    filesystem_configuration=AnonymousS3Configuration(),
    quote_character='"',
    escape_character='"'
)
```

<Callout icon="📘" theme="info">
  **Note** - _**Public S3 bucket access**_

  When using public any bucket such as `jfrogml-public`, `nyc-tlc` , etc.. , use the `AnonymousS3Configuration` to access without credentials as shown in the example.
</Callout>

<Callout icon="❗️" theme="error">
  **Important**

  Default timestamp format for `date_created_column` in CSV files should be `yyyy-MM-dd'T'HH:mm:ss`, optionally with `[.SSS][XXX]`.

  For example `2020-01-01T00:00:00`
</Callout>

##### Accessing Private Amazon S3 Buckets in Data Sources

To securely leverage data stored in Amazon S3 buckets within the JFrog ML feature store, we support two robust authentication methods. This guide provides a comprehensive overview of setting up access to private S3 buckets, ensuring that your data remains secure while being fully accessible for your data operations.

1. **IAM Role ARN Based Authentication**

   This method allows JFrog ML to assume an IAM role with permissions to access your S3 bucket. Create an IAM role in AWS with the necessary permissions to access the S3 bucket. For a step-by-step guide, refer to <Anchor label="Configuring IAM Roles for S3 Access" title="Accessing AWS Resources with IAM Role" href="/docs/accessing-aws-resources-with-iam-role">Configuring IAM Roles for S3 Access</Anchor>.

   ```
   from frogml.core.feature_store.data_sources.source_authentication import AwsAssumeRoleAuthentication

   aws_authentication = AwsAssumeRoleAuthentication(role_arn='<YOUR_IAM_ROLE_ARN') f
   ```
2. **Credentials Based Authentication**

   For scenarios where IAM role-based access isn't preferred, use your AWS access and secret keys, stored securely in the JFrog ML Secrets Management Service. Save your AWS `access_key` and `secret_key` in <Anchor label="JFrog ML Secret Management" title="Secret Management" href="/docs/secret-management">JFrog ML Secret Management</Anchor>.

   ```
   from frogml.core.feature_store.data_sources.source_authentication import AwsCredentialsAuthentication

   aws_authentication = AwsCredentialsAuthentication(access_key_secret_name='your-access-key-frogml-secret', 
                                                     secret_key_secret_name='your-secret-key-frogml-secret')
   ```

After setting up your authentication method, use the `aws_authentication` object to configure your CSV or Parquet data source, by assigning it to the `filesystem_configuration` parameter, as in the example below:

```
from frogml.feature_store.data_sources.batch.csv import CsvSource
from frogml.feature_store.data_sources.batch.parquet import ParquetSource

csv_source = CsvSource(
   name='name_with_underscores',
   description='',
   date_created_column='your_date_related_column',      
   path='s3://s3...',
   quote_character="'",
   escape_character="\\",
   filesystem_configuration= aws_authentication
)
```

#### Redshift

In order to connect to Redshift source, you will need to grant access either using AWS Access Key & Secret Key or using IAM Role.

```
from frogml.feature_store.data_sources import RedshiftSource

redshift_source = RedshiftSource(
    name='my_source',
    date_created_column='DATE_CREATED',
    description='Some Redshift Source',
    url="company-redshift-cluster.xyz.us-east-1.redshift.amazonaws.com:5439/DBName",
    db_table='my_table',
    query='base query when fetching data from Redshift', # Must choose either db_table or query
    iam_role_arn='arn:aws:iam::123456789:role/assumed_role_redshift',
    db_user='dbuser_name',
)
```

#### MySQL

```
from frogml.feature_store.data_sources import MysqlSource

mysql_source = MysqlSource(
    name='mysql_source',
    description='a mysql source description',
    date_created_column='date_created',
    username_secret_name='jfrogml_secret_mysql_user', # uses the JFrogML Secret Service
    password_secret_name='jfrogml_secret_mysql_pass', # uses the JFrogML Secret Service
    url='<MysqlAddress/DNS:Port>',
    db_table='db.table_name',  # i.e database1.table1
    query='base query when fetching data from mysql'  # Must choose either db_table or query
)
```

##### Postgres

```
from frogml.feature_store.data_sources.batch.postgres import ProtoPostgresqlSource

postgres_source = ProtoPostgresqlSource(
    name='postgresql_source',
    description='a postgres source description',
    date_created_column='date_created',
    username_secret_name='jfrogml_secret_postgres_user', # uses the JFrog ML Secret Service
    password_secret_name='jfrogml_secret_postgres_pass', # uses the JFrog ML Secret Service
    url='<PostgresqlAddress/DNS:Port/DBName>',
    db_table='schema.table_name',  # default schema: public
    query='base query when fetching data from postgres'  # Must choose either db_table or query
)
```

#### Clickhouse

```
from frogml.feature_store.data_sources import ClickhouseSource

clickhouse_source = ClickhouseSource(
    name='clickhouse_source',
    description='a clickhouse source description',
    date_created_column='date_created', # Has to be of format DateTime64
    username_secret_name='jfrogml_secret_clickhouse_user', # uses the JFrog ML Secret Service
    password_secret_name='jfrogml_secret_clickhouse_pass', # uses the JFrog ML Secret Service
    url='<ClickhouseAddress/DNS:Port/DBName>', # datatabase name is optional
    db_table='database_name.table_name',  # default database: default
    query='base query when fetching data from clickhouse'  # Must choose either db_table or query
)
```

#### Vertica

```
from frogml.feature_store.data_sources import VerticaSource

vertica_source = VerticaSource(
    name='vertica_source',
    description='a vertica source description',
    date_created_column='date_created',
    username_secret_name='jfrogml_secret_vertica_user', # uses the JFrog ML Secret Service
    password_secret_name='jfrogml_secret_vertica_pass', # uses the JFrog ML Secret Service
    host='VerticaHost without :port suffix',
    port=5444,
    database='MyVerticaDatabase',
    schema='MyVerticaSchema e.g: public',
    table='table_name'
)
```

#### AWS Athena

The Athena source is used to connect JFrog ML to Amazon Athena, allowing users to query and ingest data seamlessly

```
from frogml.feature_store.data_sources.batch.athena import AthenaSource
from frogml.core.feature_store.data_sources.source_authentication import AwsAssumeRoleAuthentication
from frogml.core.feature_store.data_sources.time_partition_columns import DatePartitionColumns

athena_source = AthenaSource(
    name='my_athena_source',
    description='my Athena source description',
    date_created_column='date_created',
    aws_region='us-east-1',
    s3_output_location='s3://some-athena-queries-bucket/',
    workgroup='some-workgroup',
    query='SELECT * FROM "db"."table"',
    aws_authentication=AwsAssumeRoleAuthentication(role_arn='some_role_arn'),
    time_partition_columns=DatePartitionColumns(date_column_name='date_pt', date_format='%Y%m%d'),
)
```

<Callout icon="📘" theme="info">
  **Note** - _**Workgroups**_

  By default, your default workgroup in Athena is called `primary`. However, for optimal organization and resource management, it's recommended to establish a dedicated workgroup specifically for handling FeatureSet-related queries. This separation ensures that queries related to the JFrog ML FeatureSets are isolated from other users or applications utilizing AWS Athena, allowing for better debugging, query prioritization, and enhanced governance.
</Callout>

##### The data source configuration supports 2 ways of authenticating to AWS Athena

`aws_authentication: AwsAuthentication`

* **Description:** Authentication method to be used.
* **Mandatory:** Yes
* **Options:**

  * `AwsAssumeRoleAuthentication`

    * **Description:** Authentication using assumed role.
    * **Fields:**

      * `role_arn: str`: Mandatory
    * **Example:**

      ```
      from frogml.core.feature_store.data_sources.source_authentication import AwsAssumeRoleAuthentication

      aws_authentication = AwsAssumeRoleAuthentication(role_arn='some_role_arn')
      ```
  * `AwsCredentialsAuthentication`

    * **Description:** Authentication using AWS credentials in JFrog ML secrets.
    * **Fields:**

      * `access_key_secret_name: str`: Mandatory
      * `secret_key_secret_name: str`: Mandatory
    * **Example:**

      ```
      from frogml.core.feature_store.data_sources.source_authentication import AwsCredentialsAuthentication

      aws_authentication = AwsCredentialsAuthentication(access_key_secret_name='your-access-key-frogml-secret', 
                                                        secret_key_secret_name='your-secret-key-frogml-secret')
      ```

##### Define Date Partition Columns (Optional)

`time_partition_columns: TimePartitionColumns`

* **Description:** Define date partition columns correlated with `date_created_column`.
* **Optional:** Yes (Highly recommended)
* **Options:**

  * `DatePartitionColumns`

    * **Fields:**

      * `date_column_name: str`: Mandatory
      * `date_format: str`: Mandatory
    * **Example:**

      ```
      from frogml.core.feature_store.data_sources.time_partition_columns import DatePartitionColumns

      time_partition_columns = DatePartitionColumns(date_column_name='date_pt', date_format='%Y%m%d')
      ```
  * `TimeFragmentedPartitionColumns`

    * **Fields:**

      * `year_partition_column: YearFragmentColumn`: Mandatory
      * `month_partition_column: MonthFragmentColumn`: Optional (Must be set if `day_partition_column` is set)
      * `day_partition_column: DayFragmentColumn`: Optional
    * **Examples:**

      * For `year=2022/month=01/day=05`:

        ```
        from frogml.core.feature_store.data_sources.time_partition_columns import (
            ColumnRepresentation,
            TimeFragmentedPartitionColumns,
            YearFragmentColumn,
            MonthFragmentColumn,
            DayFragmentColumn,
        )
        time_partition_columns = TimeFragmentedPartitionColumns(
            YearFragmentColumn("year", ColumnRepresentation.NumericColumnRepresentation),
            MonthFragmentColumn("month", ColumnRepresentation.NumericColumnRepresentation),
            DayFragmentColumn("day", ColumnRepresentation.NumericColumnRepresentation),
        )
        ```
      * For `year=2022/month=January/day=5`:

        ```
        from frogml.core.feature_store.data_sources.time_partition_columns import (
            ColumnRepresentation,
            DayFragmentColumn,
            MonthFragmentColumn,
            TimeFragmentedPartitionColumns,
            YearFragmentColumn,
        )

        time_partition_columns = TimeFragmentedPartitionColumns(
            YearFragmentColumn("year", ColumnRepresentation.NumericColumnRepresentation),
            MonthFragmentColumn("month", ColumnRepresentation.TextualColumnRepresentation),
            DayFragmentColumn("day", ColumnRepresentation.NumericColumnRepresentation),
        )
        ```

#### Unity Catalog

An example of how to use the SDK for a Unity Catalog source:

In order to create a Unity Catalog connection, before creating a connector make sure you have the following:

* Personal access token. See <Anchor label="How to create a personal access token" target="_blank" href="https://learn.microsoft.com/en-us/azure/databricks/dev-tools/auth/pat">How to create a personal access token</Anchor>.
* Connectivity between JFrog ML environment and Unity Catalog host.
* <Anchor label="Configure access using the endpoint /api/2.1/unity-catalog" target="_blank" href="https://learn.microsoft.com/en-us/azure/databricks/external-access/unity-rest">Configure access using the endpoint /api/2.1/unity-catalog</Anchor>.

There are two distinct ways to use the Unity Catalog connector:

Providing `query`.

```
unitycatalog_source = UnityCatalogSource(
    name="my_source",
    description="some unity catalog data source",
    date_created_column="DATE_CREATED",
    uri="https://<Databricks Address>/api/2.1/unity-catalog",
    catalog="unity_catalog_name",
    schema="schema_name",
    query="select * from table_name",
    personal_access_token_secret_name="jfrogml_secret_unity_catalog_pat_token", # use secret service
)
```

Providing `table`.

```
unitycatalog_source = UnityCatalogSource(
    name="my_source",
    description="some unity catalog data source",
    date_created_column="DATE_CREATED",
    uri="https://<Databricks Address>/api/2.1/unity-catalog",
    catalog="unity_catalog_name",
    schema="schema_name",
    table="table_name",
    personal_access_token_secret_name="jfrogml_secret_unity_catalog_pat_token", # use secret service
)
```

**Limitation:** Feature sets cannot use multiple Unity Catalog data sources if they are configured to different catalogs that share the same name.

## Streaming Data Sources

### Kafka Source

```
from frogml.feature_store.data_sources import KafkaSource

kafka_source = KafkaSource(name="sample_source",
                             description="Sample Source",
                             bootstrap_servers="broker.foo.bar.com:9094, broker2.foo.bar.com:9094",
                             subscribe="sample_topic",
                             deserialization=deserializer)
```

The acceptable parameters for KafkaSource are:

| parameter           | type           | description                                                                                                                                                                                                                                                                             | default value                                                                        |
| :------------------ | :------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------- |
| bootstrap_servers   | str            | comma-separated sequence of host:port entries                                                                                                                                                                                                                                           | This parameter is mandatory                                                          |
| deserialization     | Deserializer   | Deserializer to use                                                                                                                                                                                                                                                                     | This parameter is mandatory                                                          |
| secret_configs      | Dict[str, str] | Configurations that will be injected to the reader, where the value will be resolved from a JFrog ML Secret with the same name, that is, k:v will be resolved to k:get_secret(V) before using this for credentials, see other mechanisms (SASL/SCRAM) that are supported out-of-the-box | `{}`                                                                                 |
| passthrough_configs | Dict[str, str] | Configurations that will be injected to the reader w/o resolving to a JFrog ML Secret. DO NOT PLACE CREDENTIALS HERE!                                                                                                                                                                   | `{}`                                                                                 |
| subscribe           | str            | comma separated list of 1 or more topics                                                                                                                                                                                                                                                | No default value, exactly one `{subscribe, assign, subscribe\_pattern}` is to be set |
| assign              | str            | JSON string, where each key is a topic name and the value is an array of partition numbers to subscribe to                                                                                                                                                                              | No default value, exactly one `{subscribe, assign, subscribe\_pattern}` is to be set |
| subscribe_pattern   | str            | Java regex that matches the topics to read from                                                                                                                                                                                                                                         | No default value, exactly one `{subscribe, assign, subscribe\_pattern}` is to be set |
| description         | str            | Description of the source                                                                                                                                                                                                                                                               | empty string                                                                         |
| name                | str            | name of the source. this is the View Name with which this data source will appear in the Transformation definition (see below)                                                                                                                                                          | This parameter is mandatory                                                          |

### Deserialization

The Kafka streaming data source currently supports 2 types of message deserializer:

#### Generic Deserializer

<Callout icon="📘" theme="info">
  **Note**

  Generic Deserializer

  * Supports `AVRO` and `JSON` formats.
  * Assumes the message data is stored under `value` field
  * compatible data types are in accordance to spark data types
</Callout>

#### Using JSON

When using JSON the schema is in the Spark-proprietary JSON schema definition (**NOT JSON Schema**):

```
{
  "type": "struct",
  "fields": [
    {
      "metadata": {},
      "name": "timestamp",
      "nullable": true,
      "type": "string"
    },
    {
      "metadata": {},
      "name": "key",
      "nullable": true,
      "type": "integer"
    },
    {
      "metadata": {},
      "name": "value",
      "nullable": true,
      "type": "integer"
    }
  ]
}
```

```
from frogml.feature_store.data_sources import KafkaSource, MessageFormat, GenericDeserializer

deserializer = GenericDeserializer(
    message_format=MessageFormat.JSON, schema=<spark_json_schema_definition>
)

kafka_source = KafkaSource(name="sample_source",
                             description="Sample Source",
                             bootstrap_servers="broker.foo.bar.com:9094, broker2.foo.bar.com:9094",
                             subscribe="sample_topic",
                             deserialization=deserializer)
```

<Callout icon="❗️" theme="error">
  **Important** - _**Behavior for Invalid Messages**_

  When using `GenericDeserializer` for JSON, a message can be invalid when:

  * The value of the message in Kafka is not a UTF8 string.
  * The value of the message in Kafka is a UTF8 string but is not a valid JSON string.
  * The value of the message in Kafka is a valid JSON string but does not follow the schema in one or more of its fields.

  When a message is invalid it might cause an entire record to have null values, or just specific fields of the record to have a null value. Invalid messages will always create a record in the Data Source and will fail silently.
</Callout>

#### Using Avro

When using Avro the schema is an <Anchor label="Avro schema" target="_blank" href="https://avro.apache.org/docs/1.12.0/specification/">Avro schema</Anchor> :

```
from frogml.feature_store.data_sources import KafkaSource, MessageFormat, GenericDeserializer

deserializer = GenericDeserializer(
    message_format=MessageFormat.AVRO, schema=<avro_schema>
)

kafka_source = KafkaSource(name="sample_source",
                             description="Sample Source",
                             bootstrap_servers="broker.foo.bar.com:9094, broker2.foo.bar.com:9094",
                             subscribe="sample_topic",
                             deserialization=deserializer)
```

<Callout icon="❗️" theme="error">
  **Important** - _**Behavior for Invalid Messages**_

  When working with Avro it is important to validate the input to your topic properly. When using `GenericDeserializer` for Avro, invalid Avro messages can cause unexpected results. Unlike the behavior for JSON - some failures are not silent and may cause the entire Feature Set ingestion to fail. Even when the failure is silent it may cause bad data.
</Callout>

#### Custom Deserializer

<Callout icon="📘" theme="info">
  **Note** - **_Custom Deserializer_**

  Specifies how messages should be deserialized - in our case, the messages were in JSON format, and contained 3 fields: `timestamp`, `full_name` and `address` and were stored in the `value` field.

  When specifying a deserializer, any arbitrary python function that accepts a PySpark `DataFrame` and returns a `DataFrame` can be specified, under several conditions:

  1. Row-Level transformations only.
  2. Must not return an input that is detached from the original `DataFrame` (e.g., do not use the rdd, do not create a new `DataFrame` etc.) - this will break the streaming graph.
  3. The schema of the input `DataFrame` will always be the same, regardless of any other kafka configuration, see table below.
</Callout>

```python
from frogml.feature_store.data_sources import KafkaSource, CustomDeserializer
from pyspark.sql.functions import *
from pyspark.sql.types import *


def deser_function(df):
    schema = StructType(
        [
            StructField("timestamp", TimestampType()),
            StructField("full_name", IntegerType()),
            StructField("address", IntegerType()),
        ]
    )
    deserialized = df.select(
        from_json(col("value").cast(StringType()), schema).alias("data")
    ).select(col("data.*"))

    return deserialized


deserializer = CustomDeserializer(f=deser_function)


kafka_source = KafkaSource(name="sample_source",
                             description="Sample Source",
                             bootstrap_servers="broker.foo.bar.com:9094, broker2.foo.bar.com:9094",
                             subscribe="sample_topic",
                             deserialization=deserializer)
```

Built-in Columns when accessing a Kafka Topic:

| Column Name        | Type      |
| ------------------ | --------- |
| key                | binary    |
| value              | binary    |
| topic              | string    |
| partition          | int       |
| offset             | long      |
| timestamp          | timestamp |
| timestampType      | int       |
| headers (optional) | array     |
