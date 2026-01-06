---
title: Feature Consumption
deprecated: false
hidden: false
metadata:
  title: Feature Consumption
  description: 'This section reviews the following topics:'
  legacyUUIDs:
    - UUID-cbe48041-bfd5-9782-afe0-540667a114cf
    - UUID-274f8721-4a82-207b-e78c-e6b4b1ba10be
    - UUID-76b162d9-b9e2-2015-c662-cb32235ccd09
    - UUID-e2f8d6c0-67f0-17c6-3db0-e3d8653ba85b
    - UUID-ff123923-0891-b9ea-849b-edc90c509ac0
    - UUID-3f348dc3-c491-5ce1-2a80-8242a2db41a3
  robots: index
---
This section reviews the following topics:

[Features in Inference](/docs/feature-consumption#features-in-inference)

[Features in Training](/docs/feature-consumption#features-in-training)

## Features in Inference

This tutorial shows how to access data stored in the JFrog ML Feature Store online store during the inference.

### Using `OnlineClient`

In the predict function, we create an instance of the `OnlineClient`.

After that, we create a `ModelSchema` containing all of the features we want to retrieve.

We create a DataFrame containing the entities identifiers and pass it to <Anchor label="the Feature Store" target="_blank" href="https://jfrog.com/blog/what-is-a-feature-store-in-ml-and-do-i-need-one/">the Feature Store</Anchor>. As a response, we get a Pandas DataFrame with the requested features.

```
import pandas as pd
from frogml.feature_store.online.client import OnlineClient
from frogml.sdk.model.schema_entities import FeatureStoreInput
from frogml.sdk.model.schema import ModelSchema

model_schema = ModelSchema(
    inputs=[
        FeatureStoreInput(name='user-credit-risk-features.checking_account'),
        FeatureStoreInput(name='user-credit-risk-features.age'),
        FeatureStoreInput(name='user-credit-risk-features.job'),
        FeatureStoreInput(name='user-credit-risk-features.duration'),
        FeatureStoreInput(name='user-credit-risk-features.credit_amount'),
        FeatureStoreInput(name='user-credit-risk-features.housing'),
        FeatureStoreInput(name='user-credit-risk-features.purpose'),
        FeatureStoreInput(name='user-credit-risk-features.saving_account'),
        FeatureStoreInput(name='user-credit-risk-features.sex'),
        FeatureStoreInput(name='liked-posts.count')
    ])

online_client = OnlineClient()

df = pd.DataFrame(columns=['user', 'post_id'],
                  data=[['06cc255a-aa07-4ec9-ac69-b896ccf05322', '1234'],
                        ['asdc255a-aa07-4ec9-ac69-b896c1231445', '7889']])

user_features = online_client.get_feature_values(model_schema, df)

print(user_features)
```

### Using the `frogml.api()` Decorator

Alternatively, we could use the features_extraction parameter and get the features automatically extracted when the Entity is being sent in your prediction input Dataframe . As with the `OnlineClient`, the `ModelSchema` is required to define what features are to be extracted from the Online Store.

```
# model.py
import frogml

@frogml.api(feature_extraction=True)
def predict(self, df, extracted_df):
  
    # Add prediction logic here
    
    return output_dataframe
```

In the predict function, use the `frogml.api()`decorator with the parameter `feature_extraction=True` as in the code example above.

The `df` Dataframe will contain your inference call inputs and the `extracted_df` Dataframe will contain the latest feature vectors from the Online store for the queried entities.

### Using the REST API

The JFrog ML Online Store can also be queried via REST calls as shown in the example below:

#### Generate a JFrog ML Token for Authentication

Use the following command to obtain a token, valid for 24 hours:

```
curl --request POST 'https://grpc.qwak.ai/api/v1/authentication/qwak-api-key' \
      --header 'Content-Type: application/json' \
      --data '{"qwakApiKey": "<QWAK_API_KEY>"}'
```

Optionally, store the token in an environment variable:

```
export JFROG_TOKEN="<OUTPUT_FROM_AUTHENTICATION_CURL_CALL>"
```

#### Retrieve Online Features

With the fresh JFrog ML token, use the following command to extract features:

```
curl --location 'https://grpc.<YOUR-ACCOUNT>.qwak.ai/api/v1/rest-serving/multiFeatureValues/' \
--header 'Authorization: Bearer '$JFROG_TOKEN'' \
--header 'Content-Type: application/json' \
--data '{
  "entitiesToFeatures": [{
    "features": [{
      "batchV1Feature": {
        "name": "user-credit-risk-features.checking_account"
      }
    }, {
      "batchV1Feature": {
        "name": "user-credit-risk-features.age"
      }
    }],
    "entityName": "user"
  }],
  "entityValuesMatrix": {
    "header": {
      "entityNames": ["user"]
    },
    "rows": [{
      "entityValues": ["45b7836f-bf7c-4039-bc9e-d33982cc1fc5"]
    }]
  }
}'
```

<Callout icon="📘" theme="info">
  When referencing feature sets in SDK or REST calls, use hyphens `-` instead of underscores `_`. This is a common notation in the JFrog ML platform to ensure consistency and avoid errors during calls.
</Callout>

<Callout icon="❗️" theme="error">
  **Important**

  If you're on a SaaS account, use `batchV1Feature` as suggested in the example above.

  For [hybrid](/docs/jfrog-ai-catalog-architecture) accounts, switch to using `batchFeature` in the REST data JSON payload.
</Callout>

Example JSON Response:

```
{
  "featureValues": "{\"index\":[0],\"data\":[[\"moderate\",27]],\"columns\":[\"user-credit-risk-features.checking_account\",\"user-credit-risk-features.age\"]}"
}
```

These examples are using `curl` for REST calls but any other REST client will work just as well.

## Features in Training

This documentation provides examples and usage patterns for interacting with the <Anchor label="Offline Feature Store" target="_blank" href="https://jfrog.com/blog/what-is-a-feature-store-in-ml-and-do-i-need-one/">Offline Feature Store</Anchor> using the `OfflineClientV2` in Python (available from SDK version 0.5.61 and higher). It covers how to retrieve feature values for machine learning model training and analysis.

### Prerequisites:

Before using these examples, ensure you have the following Python packages installed:

```
pip install pyathena pyarrow
```

### APIs:

#### Get Feature Values

This API retrieves features from an offline feature store for one or more feature sets, given a `population` DataFrame. The resulting DataFrame will include the `population` DataFrame enriched with the requested feature values as of the `point_in_time` specified.

**Arguments:**

* `features: List[FeatureSetFeatures]` - **required**

  A list of feature sets to fetch.
* `population: pd.DataFrame` - **required**

  A DataFrame containing:

  * All keys of the requested feature sets.
  * A point in time column.
  * Optional enrichments, e.g., labels.
* `point_in_time_column_name: str` - **required**

  The name of the point in time column in the `population` DataFrame.

**Returns:**`pd.DataFrame`

**Example call:**

```
import pandas as pd
from frogml.feature_store.offline import OfflineClientV2
from frogml.core.feature_store.offline.feature_set_features import FeatureSetFeatures

offline_feature_store = OfflineClientV2()

user_impressions_features = FeatureSetFeatures(
    feature_set_name='impressions',
    feature_names=['number_of_impressions']
)
user_purchases_features = FeatureSetFeatures(
    feature_set_name='purchases',
    feature_names=['number_of_purchases', 'avg_purchase_amount']
)
features = [user_impressions_features, user_purchases_features]

population_df = pd.DataFrame(
    columns=['impression_id', 'purchase_id', 'timestamp', 'label'],
    data=[['1', '100', '2021-01-02 17:00:00', 1], ['2', '200', '2021-01-01 12:00:00', 0]]
)

train_df: pd.DataFrame = offline_feature_store.get_feature_values(
    features=features,
    population=population_df,
    point_in_time_column_name='timestamp'
)

print(train_df.head())
```

**Example results:**

```
# train_df
#    impression_id   purchase_id           timestamp           label   impressions.number_of_impressions   purchases.number_of_purchases   purchases.avg_purchase_amount
# 0       1               100       2021-04-24 17:00:00       1                   312                                         76                                4.796842
# 1       2               200       2021-04-24 12:00:00       0                    86                                          5                                1.548000
```

In this example, the `label` serves as an enhancement to the dataset, rather than a criterion for data selection. This approach is particularly useful when you possess a comprehensive list of keys along with their respective timestamps. The Feature Store API is designed to cater to scenarios requiring data amalgamation from multiple feature sets, ensuring that, for each row in population_df, no more than one corresponding record is returned. Leveraging JFrog ML time-series based feature store, which organizes data within `start_timestamp` and `end_timestamp` bounds for each feature vector (key), guarantees that a singular, most relevant result is retrieved for every unique key-timestamp combination.

#### Get Feature Range Values

Retrieve features from an offline feature-set for a given time range. The result data-frame will contain all data points of the given feature-set in the given time range. If `population` is provided, then the result will be filtered by the key values it contains.

**Arguments:**

* `features: FeatureSetFeatures` - **required**:

  A list of features to fetch from a single feature set.
* `start_date: datetime` - **required**:

  The lower time bound.
* `end_date: datetime` - **required**:

  The upper time bound.
* `population: pd.DataFrame` - **optional**:

  A DataFrame containing the following columns:

  * The key of the requested feature-set **required**
  * Enrichments e.g., labels. **optional**

**Returns:**`pd.DataFrame`

**Example Call:**

```
from datetime import datetime
import pandas as pd
from frogml.feature_store.offline import OfflineClientV2
from frogml.core.feature_store.offline.feature_set_features import FeatureSetFeatures

offline_feature_store = OfflineClientV2()

start_date = datetime(year=2021, month=1, day=1)
end_date = datetime(year=2021, month=1, day=3)
features = FeatureSetFeatures(
    feature_set_name='purchases',
    feature_names=['number_of_purchases', 'avg_purchase_amount']
)

train_df: pd.DataFrame = offline_feature_store.get_feature_range_values(
    features=features,
    start_date=start_date,
    end_date=end_date
)

print(train_df.head())
```

**Example Results:**

```
# train_df
#      purchase_id           timestamp           purchases.number_of_purchases     purchases.avg_purchase_amount
# 0       1             2021-01-02 17:00:00               76                                4.796842
# 1       1             2021-01-01 12:00:00                5                                1.548000
# 2       2             2021-01-02 12:00:00                5                                5.548000
# 3       2             2021-01-01 18:00:00                5                                2.788000                        
```

<Callout icon="📘" theme="info">
  **Note**

  _**Current Limitations**_

  The get_feature_range_values API call is currently not available for Streaming Aggregations feature sets and not available to fetch data for multiple feature sets at the same time (join data).
</Callout>
