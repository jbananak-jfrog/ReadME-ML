---
title: Batch Deployments
deprecated: false
hidden: false
metadata:
  title: Batch Deployments
  description: Given a successful build, you can deploy your model as a batch application.
  legacyUUIDs:
    - UUID-75a82642-5be8-5e27-1489-aee42b6f6880
    - UUID-12d4c0e7-3bd3-f679-1303-c306f58f29df
    - UUID-d07a4461-8a25-0063-eaea-f2499c725049
    - UUID-25499b2d-d497-1322-368b-8be210e34367
    - UUID-e83ba9c3-bcef-b30a-5e5e-2d8d47d8abcc
    - UUID-17525b81-e83e-b999-cd33-7cbbe6cb9084
    - UUID-a2de47d6-19af-2414-56bd-574a3c34ed8e
    - UUID-38aa3c57-6473-a6a8-ed98-84a92a2332fc
  robots: index
---
Given a successful build, you can deploy your model as a batch application.

This deployment type enables you to run batch inference executions in the system, and handle data files from an online cloud storage provider.

## Deployment Configuration

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Default Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Model ID [**Required**]
      </td>

      <td>
        The Model ID, as displayed on the model header.
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        Build ID [**Required**]
      </td>

      <td>
        The JFrog ML-assigned build ID.
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        Initial number of pods
      </td>

      <td>
        The number of <Anchor label="k8s pods" target="_blank" href="https://kubernetes.io/docs/concepts/workloads/pods/">k8s pods</Anchor> to be used by the deployment. Each pod handles one or more files/tasks.
      </td>

      <td>
        1
      </td>
    </tr>

    <tr>
      <td>
        CPU fraction
      </td>

      <td>
        The CPU fraction allocated to each pod. The CPU resource is measured in CPU units. One CPU, in JFrog ML, is equivalent to:

        1 AWS vCPU

        1 GCP Core

        1 Azure vCore

        1 Hyperthread on a bare-metal Intel processor with Hyperthreading
      </td>

      <td>
        2
      </td>
    </tr>

    <tr>
      <td>
        Memory
      </td>

      <td>
        The RAM memory (in MB) to allocate to each pod.
      </td>

      <td>
        512
      </td>
    </tr>

    <tr>
      <td>
        IAM role ARN
      </td>

      <td>
        The user-provided AWS custom IAM role.
      </td>

      <td>
        None
      </td>
    </tr>

    <tr>
      <td>
        GPU Type
      </td>

      <td>
        The GPU Type to use in the model deployment. Supported options are, NVIDIA K80, NVIDIA Tesla V100, NVIDIA T4 and NVIDIA A10.
      </td>

      <td>
        None
      </td>
    </tr>

    <tr>
      <td>
        GPU Amount
      </td>

      <td>
        The number of GPUs available for the model deployment.

        Varies based on the selected GPU type.
      </td>

      <td>
        Based on GPU Type
      </td>
    </tr>

    <tr>
      <td>
        Purchase Option
      </td>

      <td>
        Choose between `on-demand` or `spot` instances for the batch executions.
      </td>

      <td>
        None (spot)
      </td>
    </tr>

    <tr>
      <td>
        Service Account Key Secret Name
      </td>

      <td>
        The service account key secret name to reach Google cloud services.
      </td>

      <td>
        None
      </td>
    </tr>
  </tbody>
</Table>

## Batch Deployment from the UI

To deploy a batch model from the UI:

1. In the left navigation bar in the JFrog ML UI, select **Models** and select a model to deploy.
2. Select the **Builds** tab. Find a build to deploy and click the deployment toggle. The **Deploy** dialog box appears.
3. Select **Batch** and then select **Next**.

## Batch Deployment from the CLI

To deploy a model in batch mode from the CLI, populate the following command template:

```
frogml models deploy batch \
    --model-id <model-id> \
    --build-id <build-id> \
    --pods <pods-count> \
    --cpus <cpus-fraction> \
    --memory <memory-size>
```

For example, for the model built in the <Anchor label="Get Started with JFrog ML" title="Get Started with JFrog ML" href="/docs/get-started-with-jfrog-ml">Get Started with JFrog ML</Anchor> section, the deployment command is:

```
frogml models deploy batch \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --pods 4 \
    --cpus 3 \
    --memory 1024
```

## Storage-Based Execution

The low-level API of the batch execution allows you to start and execution from a specific remote path, and write the results to another path.

Each file in the input path is translated to a single unit of processing (a JFrog ML task). Each input file is read and passed as a single chunk to the model `predict` function.

<Callout icon="⚠️" theme="warning">
  **Warning**

  _**Task Processing**_

  As the file is transformed to a single prediction request, you must ensure that the model is deployed on an instance with sufficient resources to handle the batch request.

  Where batch predictions are resource-heavy, consider splitting the input dataset into multiple smaller files.
</Callout>

#### Execution Configuration

| Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                            | Default Value                                           |
| :-------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ |
| Model ID [**Required**]           | The Model ID, as displayed on the model header.                                                                                                                                                                                                                                                                                                                        |                                                         |
| Build ID                          | The JFrog ML-assigned build ID. You can optionally add this to use a different build in order to perform the execution.                                                                                                                                                                                                                                                |                                                         |
| Bucket                            | The source and destination bucket. If you read and write to the same bucket, you can specify this, but it is not mandatory. Note: This parameter is **required** if 'Source Bucket' and 'Destination Bucket' are not set.                                                                                                                                              |                                                         |
| Source bucket                     | The bucket from which the input files are read, to start an execution. Note: This parameter is **required** if 'Bucket' is not set.                                                                                                                                                                                                                                    |                                                         |
| Destination bucket                | The bucket into which the execution output files are written. Note: This parameter is **required** if 'Bucket' is not set.                                                                                                                                                                                                                                             |                                                         |
| Source folder [**Required**]      | The path to the source bucket where all the inference files are located.                                                                                                                                                                                                                                                                                               |                                                         |
| Destination Folder [**Required**] | The path to the destination bucket where the result files are stored.                                                                                                                                                                                                                                                                                                  |                                                         |
| Input File Type                   | The file types supported by JFrog ML. The supported formats are: CSV, Parquet and Feather.                                                                                                                                                                                                                                                                             | CSV                                                     |
| Output File Type                  | The types of the files stored by JFrog ML. The supported formats are: CSV, Parquet and Feather.                                                                                                                                                                                                                                                                        | CSV                                                     |
| Access Token Name                 | The name of the secret (created using our Secret Service) that contains the Access Token with permission to the source and destination buckets.                                                                                                                                                                                                                        |                                                         |
| Access Secret Name                | The name of the secret (created using our Secret Service) that contains the Access Secret with permission to the source and destination buckets.                                                                                                                                                                                                                       |                                                         |
| Job Timeout                       | The job timeout, in seconds. By setting the job timeout, you will limit the execution time, and it will fail if not completed in time.                                                                                                                                                                                                                                 | 0 - No Timeout                                          |
| File Timeout                      | A single file timeout, in seconds. Setting this will limit the processing time for a single file, and fail the entire execution if one of the files does not finish in time.                                                                                                                                                                                           | 0 - No Timeout                                          |
| IAM role ARN                      | The user-provided AWS custom IAM role.                                                                                                                                                                                                                                                                                                                                 | None                                                    |
| Pods                              | The number of <Anchor label="k8s pods" target="_blank" href="https://kubernetes.io/docs/concepts/workloads/pods/">k8s pods</Anchor> which will be used at batch inference time. Number of pods sets the maximum parallelism for an inference job. Each pod handles one or more files/tasks. This configuration takes precedence over the deployed model configuration. | The number of executors/pods defined in the deployment. |
| Instance                          | The relevant [Instance](/docs/instance-sizes-ml-credits#general-purpose-instances) to run the batch operation.                                                                                                                                                                                                                                                         | Small                                                   |
| Parameters                        | A list of parameters expressed as key-value pairs which will be passed to the execution request. The parameters are passed to the inference container as **environment variables**.                                                                                                                                                                                    |                                                         |
| service account key secret name   | Gcp service account key name to reach google cloud provider.                                                                                                                                                                                                                                                                                                           | None                                                    |

#### Running Batch Execution Using S3 Buckets

Currently, only **S3** buckets situated in the region configured for your JFrog ML environment are compatible as sources for input / output paths.

If you intend to employ a different IAM Role ARN to grant permissions to an S3 location, you must include the provided trust policy. For customized parameters, please reach out to our support team.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::<AWS_ACCOUNT_ID>:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/<OIDC_EKS_CLUSTER_ID>"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "oidc.eks.us-east-1.amazonaws.com/id/<OIDC_PROVIDER_EKS_CLUSTER_ID>:aud": "sts.amazonaws.com"
                },
                "ForAnyValue:StringEquals": {
                    "oidc.eks.us-east-1.amazonaws.com/id/<OIDC_PROVIDER_EKS_CLUSTER_ID>:sub": [
                        "system:serviceaccount:qwak:kube-deployment-captain-access"
                    ]
                }
            }
        }
    ]
}
```

#### Using Custom AWS IAM Role

Custom IAM Role allow access to both private buckets and source / destination folders. Provide the custom IAM role name when calling a batch execution.

The IAM role should be created with the following trust policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "ArnLike": {
          "aws:PrincipalArn": "arn:aws:iam::<account-id>:role/qwak-eks-base*"
        }
      }
    }
  ]
}
```

#### Batch Execution

To start an execution from the SDK, use the following command:

```
from frogml.core.clients.batch_job_management.client import BatchJobManagerClient
from frogml.core.clients.batch_job_management.results import StartExecutionResult
from frogml.core.clients.batch_job_management.executions_config import ExecutionConfig

// The execution configuration
execution_spec = ExecutionConfig.Execution(
    model_id=<model-id>,
    source_bucket=<source-bucket-name>,
    destination_bucket=<destination-bucket-name>,
    source_folder=<source-folder-path>,
    destination_folder=<destination-folder-path>,
    input_file_type=<input-file-type>,
    output_file_type=<output-file-type>,
    access_token_name=<access_token_name>,
    access_secret_name=<access-secret-name>,
    job_timeout=<job-timeout>,
    file_timeout=<file-timeout>,
    parameters=<dictionary of user provided paramaters>
)

resources_config = ExecutionConfig.Resources(
    pods=<number-of-pods>,
    instance=<instance-type>,
)

execution_config = ExecutionConfig(execution=execution_spec, resources=resources_config)
batch_job_manager_client = BatchJobManagerClient()

execution_result: StartExecutionResult = batch_job_manager_client.start_execution(execution_config)
execution_id = execution_result.execution_id
```

```
frogml models execution start \                                                                                                                             TERM ✘  8m 34s   base   12:58:22 
    --model-id <model-id> \
    --source-bucket <source-bucket-name> \
    --source-folder <source-folder-path> \
    --destination-bucket <destination-bucket-name> \    
    --destination-folder <destination-folder-path> \
    --input-file-type <input-file-type> \
    --output-file-type <output-file-type> \
    --access-token-name <buckets-access-token-secret-name> \
    --access-secret-name <buckets-access-secret-secret-name> \
    --job-timeout <entire-job-timeout-in-seconds> \
    --file-timeout <single-file-timeout-in-seconds> \
    --pods <pods-count> \
    --cpus <cpus-fraction> \
    --memory <memory-size> \
    --build-id <alternate-build-id>
```

Here is a simplified version with all the default values:

```
from frogml.core.clients.batch_job_management.client import BatchJobManagerClient
from frogml.core.clients.batch_job_management.results import StartExecutionResult
from frogml.core.clients.batch_job_management.executions_config import ExecutionConfig

// The execution configuration
execution_spec = ExecutionConfig.Execution(
    model_id=<model-id>,
    bucket=<bucket-name>,
    destination_bucket=<destination-bucket-name>,
    source_folder=<source-folder-path>,
    destination_folder=<destination-folder-path>,
    access_token_name=<access_token_name>,
    access_secret_name=<access-secret-name>
)

execution_config = ExecutionConfig(execution=execution_spec)
batch_job_manager_client = BatchJobManagerClient()

execution_result: StartExecutionResult = batch_job_manager_client.start_execution(execution_config)
execution_id = execution_result.execution_id
```

```
frogml models execution start \                                                                                                                             TERM ✘  8m 34s   base   12:58:22 
    --model-id <model-id> \
    --bucket <bucket-name> \
    --source-folder <source-folder-path> \
    --destination-folder <destination-folder-path> \
    --access-token-name <buckets-access-token-secret-name> \
    --access-secret-name <buckets-access-secret-secret-name>
```

#### Batch Job Parallelism

Note that every file in the given input path is considered a task. A task is the main unit of parallelism for a batch execution job.

For example, if five pods are requested during the batch execution (whether specified by the deployment or in the batch execution job itself), and 10 files need to processed, five files (or tasks) are executed in parallel, out of the 10 tasks that comprise the batch job.

For this reason, there is no point in requesting more pods than the number of files which need to be processed.

<Callout icon="📘" theme="info">
  **Note**

  _**Concurrent Executions**_

  You can run multiple executions, concurrently. The only limitation is not running executions with identical values for the following parameters:

  1. Model ID
  2. Build ID
  3. Source Bucket
  4. Source Folder
  5. Destination Bucket
  6. Destination Folder

  The assumption is that running two executions with the same parameters, is redundant.
</Callout>

#### Switching Between On-demand and Spot Instances

It is possible to choose a specific instance type per batch execution, overriding the configuration stated at the currently deployed models.

By providing the purchase option as advanced options, you may choose the instance type for a specific batch execution.

When not providing a specific option, the option stated in the currently deployed build will be taken.

```
from frogml.core.clients.batch_job_management.executions_config import ExecutionConfig

# Using an on-demand instance for this execution
execution_spec = ExecutionConfig.Execution(
    ...
    advanced_options=ExecutionConfig.AdvancedOptions(
      purchase_option="on-demand",
    )
)

# Using an spot instance for this execution
execution_spec = ExecutionConfig.Execution(
    ...
    advanced_options=ExecutionConfig.AdvancedOptions(
      purchase_option="spot",
    )
)
```

#### Local File Mode

It's also possible to run a batch execution using files stored locally.

#### SDK

The local file mode can be started by either using the `local_file_run` function from the `BatchInferenceClient`:

```
from frogml_inference.batch_client.batch_client import BatchInferenceClient

client = BatchInferenceClient()

client.local_file_run(
  model_id=...,
  source_folder=...,
  destination_folder=...,
  input_file_type=...,
  output_file_type=...,
  job_timeout=...,
  task_timeout=...,
  executors=...,
  instance=...,
  iam_role_arn=...,
  build_id=...,
  parameters=...,
  instance=...,
)
```

Parameters have the same meaning as in the execution configuration above. The source folder and the destination folder must start with `file://` prefix.

It's also required to provide the input file type that will be used to select files from the source folder (by file extension).

<Callout icon="❗️" theme="error">
  **Important**

  _**Local File Mode Parameters**_

  Remember that the source and destination folder paths MUST start with the `file://` prefix.

  `model_id`, `source_folder`, `destination_folder`, and `input_file_type` are required parameters.
</Callout>

<Callout icon="❗️" theme="error">
  **Important**

  _**Dependencies**_

  The local file mode requires additional dependencies that can be installed using `pip install "frogml-inference[batch,feedback]"`

  If the dependencies are missing, the SDK will display the following error: `Notice that BatchInferenceClient and FeedbackClient are not available in the skinny package. In order to use them, please install them as extras: pip install "frogml-inference[batch,feedback]"`.
</Callout>

<Callout icon="⚠️" theme="warning">
  **Warning**

  _**Output Directory**_

  If the destination directory doesn't exist, it will be created.

  If the destination directory exists and contains files with the same names as the ones created by the batch job, those files **WILL BE OVERWRITTEN**!
</Callout>

#### CLI

Alternatively, we can run the same local file mode using the CLI.

In this case, we run the `frogml models execution start` command with the source folder and destination folder beginning with the `file://` prefix.

The meaning of other parameters is the same as in the execution configuration above. The local file mode started with CLI requires the same parameters and dependencies as the local file mode started with an SDK. See the warnings above.

Example: `frogml models execution start --model-id the_model_id --source-folder file://path_to_a_directory --destination-folder file://path_to_output_directory --input-file-type csv` .

#### REST API

Use the following curl command template to send requests to the Batch Job Manager. Replace `<your-environment>` with the name of your account, which can be found in the bottom left corner of the JFrog ML Dashboard, and fill in the batch job details as per your needs.

```
curl --location --request POST '[https://grpc.](https://grpc.)<your-environment>.qwak.ai/api/v1/batch-job/start-job'  
--header 'Content-Type: application/json'  
--header "Authorization: Bearer $JFROG_TOKEN"  
--data '{
    "model_id": "<your_model_id>",
    "source_bucket": "...",
    "destination_bucket": "...",
    "source_folder": "batch_execution/my_model/input",
    "destination_folder": "batch_execution/my_model/output",
    "batch_job_deployment_size": {
        "number_of_pods": 4,
        "cpu": 1.0,
        "memory_amount": 512,
        "memory_units": "MIB"
    },
    "input_file_type": "PARQUET_INPUT_FILE_TYPE",
    "output_file_type": "PARQUET_OUTPUT_FILE_TYPE",
    "advanced_deployment_options": {
        "custom_iam_role_arn": "..."}
}'
```

Ensure the header is enclosed in double quotes (") when passing the `JFROG_TOKEN` as an environment variable.

**Example Output:**

```
{
  "batch_id": "<some_execution_id>",
  "success": true,
  "failure_message": ""
}
```

<Callout icon="📘" theme="info">
  **Note**

  Before executing the curl command, ensure the `JFROG_TOKEN` environment variable is correctly set in your terminal with the value of your generated JFrog ML Token. This step is crucial for authenticating your requests to the JFrog ML Platform.
</Callout>

<br />

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

<br />

<br />

<br />
