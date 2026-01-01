---
title: Batch Execution Management
excerpt: ' Understand the various commands that help you manage and track the execution status of your batch models.'
deprecated: false
hidden: false
metadata:
  robots: index
---
## Batch Execution Status

**▶ To check the current status of a batch execution:**

```
frogml models execution status --execution-id <execution-id>
```

```
from frogml.core.clients.batch_job_management.client import BatchJobManagerClient
from frogml.core.clients.batch_job_management.results import ExecutionStatusResult

batch_job_manager_client = BatchJobManagerClient()
status_response: ExecutionStatusResult = batch_job_manager_client.get_execution_status("<execution-id>")
status = status_response.status
```

The `execution_id` is returned when an execution is created, and is also visible in the UI.

## Cancel a Batch Execution

▶ **To cancel a batch execution:**

```
frogml models execution cancel --execution-id <execution-id>
```

```
from frogml.core.clients.batch_job_management.client import BatchJobManagerClient

batch_job_manager_client = BatchJobManagerClient()
batch_job_manager_client.cancel_execution("<execution-id>")
```

## &#x20;Warmup

Use the **warmup** option when the speed of execution is critical, and the execution is a single step in a larger workflow orchestration. 

The warmup option enables you to allocate the resources for execution before the execution starts. The resources are raised and kept running until the execution itself starts. This is especially relevant when a lot of resources are required, or when reducing the running time by even 5 minutes is critical.

### Low-level API

```
from frogml.core.clients.batch_job_management.client import BatchJobManagerClient
from frogml.core.clients.batch_job_management.executions_config import ExecutionConfig

# execution configuration
execution_spec = ExecutionConfig.Execution(
    model_id=<model-id>,
    bucket=<bucket-name>,
    destination_bucket=<destination-bucket-name>,
    source_folder=<source-folder-path>,
    destination_folder=<destination-folder-path>,
    access_token_name=<access_token_name>,
    access_secret_name=<access-secret-name>,
    build_id=<alternate-build-id>
)

warmup_spec = ExecutionConfig.Warmup(
    timeout=0 # warmup timeout in seconds
)

batch_job_manager_client = BatchJobManagerClient()

execution_config = ExecutionConfig(execution=execution_spec, warmup=warmup_spec)
batch_job_manager_client = BatchJobManagerClient()
batch_job_manager_client.start_warmup_job(execution_config)
```

### DF API

```
from frogml_inference.batch_client.batch_client import BatchInferenceClient

# You can also set FROGML_MODEL_ID environment variable instead of passing it
batch_inference_client = BatchInferenceClient(model_id="<model-id>")

batch_inference_client.warmup(
    executors=<number-of-pods>,
    cpus=<number-of-cpus>,
    memory=<memory-amount>,
    timeout=<timeout-for-warmup>,
    build_id=<alternate-build-id>)
```

## Troubleshooting Executions

For each execution there are two types of logs.

1. **Execution Report**: Contains the initial request, status updates, as well as the cancel and failed requests.
2. **Model Logs**: These are available once the execution advances to the stage during which the files are processed.

To view both log types, use the following command:

```
frogml models execution report --execution-id <execution-id>
```

```
from frogml.core.clients.batch_job_management.client import BatchJobManagerClient
from frogml.core.clients.batch_job_management.results import GetExecutionReportResult

execution_report: GetExecutionReportResult = batch_job_manager_client.get_execution_report(<execution-id>)
report_records = execution_report.records
model_logs = execution_report.model_logs
```

In some cases you might want to output logs from the model itself in order to better understand the model processing behavior. In order to make the logs available, you need to use the JFrog ML Logger in your model's code.

```
from frogml.core.tools.logger import get_frogml_logger

logger = get_frogml_logger()

logger.info("your message here")
```
