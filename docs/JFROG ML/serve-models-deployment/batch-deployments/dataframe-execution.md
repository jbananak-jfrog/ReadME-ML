---
title: DataFrame Execution
excerpt: >-
  DataFrame-based execution Enables you to execute a batch job on an input
  Pandas DataFrame, and receive an output DataFrame back.
deprecated: false
hidden: false
metadata:
  robots: index
---
This process is useful when you're predicting inside a notebook or as part of another process that already has a DataFrame ready. Behind the scenes, we take the DataFrame, transform it, upload the files to the cloud, and run the execution for you, waiting for a successful response.

<Callout icon="❗️" theme="error">
  **Important**

  _**Output Row Order**_

  The order of the results DataFrame is not guaranteed to be in the order of the input DataFrame. If the ordering is critical, consider adding a column you can sort on in the results DataFrame.
</Callout>

## Installing the `frogml-inference` SDK

To enable batch processing of data from your local computer using Python Dataframes with the `frogml-inference` client, you need to install additional libraries that are not included by default in the client. Therefore, it is necessary to install the `[batch]` version of the package to ensure the successful execution of data batches.

```
pip install frogml-inference[batch]
```

## Execution Example

```
from frogml_inference import BatchInferenceClient

# You can also set the FROG_MODEL_ID environment variable instead of passing it
batch_inference_client = BatchInferenceClient(model_id=<model-id>)

# You should pass the DataFrame and batch size, and the others will use the deployed configuration
result_df = batch_inference_client.run(
    df, # mandatory
    batch_size=<number-of-records-in-each-batch>, # mandatory
    executors=<number-of-pods>,
    instance=<instance-type>,
    iam_role_arn=<custom-iam-role>,
    parameters=<parameters>)
```

## Batch Job Parallelism

Behind the scenes, JFrog ML's batch processing [low level](/docs/storage-based-execution) API is used.

JFrog ML splits the requested `df` into tasks according to the batch size, and the size of the requested `df`. For example, if the requested `df` has **1000** rows, and the requested batch size is **50**, then **20** FrogML tasks are executed as part of the batch job.

The parallelism, that is, how many tasks are running in parallel, is controlled by the `executors` parameter. For example, if the `executors` parameter is set to **5**, then FrogML launches **20** tasks, with **5** tasks running in parallel.

Code example:

```
import pandas as pd
import numpy as np
from frogml_inference import BatchInferenceClient

# DF with a 1000 rows
batch_df = pd.DataFrame(np.random.randint(0,10, size=(1000,2)))

batch_inference_client = BatchInferenceClient(model_id="test_model")

# Will launch 20 tasks in general, where 5 run in parallel
result_df = batch_inference_client.run(
    df=batch_df,
    batch_size=50,
    executors=5,
    instance="small",
    parameters={"attribute": "customer_a"},
)
```

The provided parameters will be available as **environment variables**, and can be accessed as in the following example:

```
import os
import frogml
import pandas as pd

@frogml.api()
def predict(self, df):
    attribute = os.getenv("attribute", "default_customer"))

    df = df.drop([attribute], axis=1)
    return pd.DataFrame(self.catboost.predict_proba(df)[:, 1], columns=['Churn_Probability'])
```
