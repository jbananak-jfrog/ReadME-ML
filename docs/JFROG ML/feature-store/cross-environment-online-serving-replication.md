---
title: Cross-environment Online Serving Replication
excerpt: To enable fast access to inference across different regions
deprecated: false
hidden: false
metadata:
  title: Cross-environment Online Serving Replication
  description: >-
    To enable fast access to inference across different regions, we are
    introducing the Cross-Environment Online Serving Replication feature.
  legacyUUIDs:
    - UUID-ae2f849d-6970-70a4-fc11-a423878c2df4
    - UUID-8106f706-76f4-79d7-1705-1514ee1b6cd8
  robots: index
---
The **Cross-Environment Online Serving Replication** feature enables fast access to inference across different regions.

## How It Works

To leverage this feature, two environments must be created in the desired regions. When a user creates a feature set in the default environment, the inference data is automatically replicated to the other region.

To access the feature set from either region, the user simply needs to add the environment name qualifier to the feature set name.

<Callout icon="❗️" theme="error">
  **Important**

  This capability is only available for hybrid environments.
</Callout>

 **Example**

For this example, assume the user attempts to access a feature set originally defined in environment A (`env.a`) from environment B:

```python
import pandas as pd
from frogml.feature_store.online.client import OnlineClient
from frogml.sdk.model.schema_entities import FeatureStoreInput, Entity
from frogml.sdk.model.schema import ModelSchema

# Define the entity
entity = Entity(name='user')

# Define the model schema with feature set from environment A
model_schema = ModelSchema(
    inputs=[
        FeatureStoreInput(name='env.a.user-credit-risk-features.checking_account', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.age', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.job', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.duration', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.credit_amount', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.housing', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.purpose', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.saving_account', entity=entity),
        FeatureStoreInput(name='env.a.user-credit-risk-features.sex', entity=entity),
    ]
)

# Initialize the online client
online_client = OnlineClient()

# Define the input DataFrame
df = pd.DataFrame(columns=['user', 'post_id'],
                  data=[['06cc255a-aa07-4ec9-ac69-b896ccf05322', '1234'],
                        ['asdc255a-aa07-4ec9-ac69-b896c1231445', '7889']])

# Fetch feature values from the replicated environment
user_features = online_client.get_feature_values(model_schema, df)

# Print the user features
print(user_features)
```

## Model Inference

To ensure that the model accesses the replicated region and achieves high performance, the model must be deployed in the other environment. Once deployed, it will automatically access the replicated data in the corresponding region, ensuring optimal inference performance without additional configuration.
