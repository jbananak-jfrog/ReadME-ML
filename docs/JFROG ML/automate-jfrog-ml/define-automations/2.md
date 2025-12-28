---
title: Automating Batch Execution
excerpt: >-
  Automating batch model inference simplifies running batch tasks on a periodic
  basis.
deprecated: false
hidden: false
metadata:
  robots: index
---
<br />

You can set up scheduled executions to dynamically process data and ensure regular updates without manual intervention, or a need to setup external scheduling tools such as Airflow.

## Configuring `BatchExecution`

Batch model execution runs a [storage-based execution](/docs/storage-based-execution) for batch deployed model.

<Callout icon="❗️" theme="error">
  A model must be deployed as **batch** before running the automation, otherwise the automation will fail.
</Callout>

Defining a batch execution automation includes two parts:

1. `BatchJobDataSpecifications` - Telling the automation where to fetch data from.
2. Optional `BatchJobExecutionSpecifications` - Defining custom deployment resources for the batch model. When not provided, the default deployed parameters will be used.

For more details on all available parameters for configuring batch model executions, please refer to [Storage-Based Execution](/docs/storage-based-execution) page.

```
from frogml.core.automations import Automation, ScheduledTrigger, \
    BatchExecution, BatchJobDataSpecifications, BatchJobExecutionSpecifications

batch_execution_automation = Automation(
    name="scheduled_batch_inference",
    model_id="my-model-id",
    trigger=ScheduledTrigger(cron="0 0 * * *"),
    action=BatchExecution(
        data_specifications=BatchJobDataSpecifications(
            access_token_secret_name="api-token",
            access_secret_secret_name="api-secret",
            source_bucket="input_s3_bucket",
            source_folder="data_folder",
            input_file_type="<csv/parquet/feather>",
            destination_bucket="output_s3_bucket",
            destination_folder="output_data_folder",
            output_file_type="<csv/parquet/feather>",
        ),
        execution_specifications=BatchJobExecutionSpecifications(
            executors=1,
            params={"key": "value"},
            instance="small",
            job_timeout=0,
            task_timeout=0,
            custom_iam_role_arn="your-iam-role-name"
        ),
        build_id="optional-batch-model-build-id"
    )
)
```

<Callout icon="📘" theme="info">
  **Note**

  _**Scheduler Timezone**_

  The default timezone for the cron scheduler is UTC.
</Callout>

## Dynamic Folder Paths

When configuring the source and destination folders to read and write data, we may define folder paths based on the runtime timestamp.

The dynamic path may include a timestamp template, which will be injected when the automation runs.

<Callout icon="📘" theme="info">
  **Note**

  The timestamp format should follow <Anchor label="**Python strftime**" target="_blank" href="https://strftime.org/">**Python strftime**</Anchor> formatting, and wrapped with curly brackets, i.e.: `{%d-%m-%Y}`
</Callout>

## Example Path Template

There are two input parameters which support the dynamic timestamp template:

* `destination_folder`
* `source_folder`

Defining `source_folder="input_folder/{%d-%m-%Y}"` will format the path based on the current timestamp:

`source_folder="input_folder/23-05-2023"`
