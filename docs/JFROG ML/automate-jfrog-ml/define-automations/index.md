---
title: Define Automations
deprecated: false
hidden: false
metadata:
  title: Define Automations
  description: >-
    JFrog ML offers built-in automations for various recurring actions in the
    model lifecycle. These automations are meant to replace external
    orchestration tools such as Airflow for example.
  legacyUUIDs:
    - UUID-3a3c5b36-c56b-5548-3ee9-cef050ab3f11
    - UUID-f73624e7-87b1-757a-8375-f9d27d65866a
    - UUID-a754760a-e389-76c8-8f9b-8425f9cd175b
    - UUID-87a44d4a-1b0d-26a1-45b9-b1370882dfb5
    - UUID-eb4aba0f-96eb-246b-b5e4-eb4a2fefefba
    - UUID-6a588ad0-3058-5bad-a7a0-0e86bdb543af
  robots: index
---
JFrog ML offers built-in automations for various recurring actions in the model lifecycle. These automations are meant to replace external orchestration tools such as Airflow for example.

In the JFrog ML platform, you can easily define automations for model retraining or batch model executions on either time based or trigger based.

## Configuring Automations

<Callout icon="⚠️" theme="warning">
  **Warning**

  The automation name must be unique throughout your models within the JFrog ML environment.
</Callout>

We now create an instance of the Automation class and configure it:

```
from frogml.core.automations import Automation, ScheduledTrigger, FrogmlBuildDeploy,\
    BuildSpecifications, BuildMetric, ThresholdDirection, DeploymentSpecifications

test_automation = Automation(
    name="retrain_my_model",
    model_id="my-model-id",
    trigger=ScheduledTrigger(cron="0 0 * * 0"),
    action=FrogmlBuildDeploy(
        build_spec=BuildSpecifications(git_uri="https://github.com/org_id/repository_name.git#dir_1/dir_2",
                                       git_access_token_secret="token_secret_name",
                                       git_branch="main",
                                       main_dir="main",
                                       tags=["prod"],
                                       env_vars=["key1=val1", "key2=val2", "key3=val3"]),
        deployment_condition=BuildMetric(metric_name="f1_score",
                                         direction=ThresholdDirection.ABOVE,
                                         threshold="0.65"),
        deployment_spec=DeploymentSpecifications(number_of_pods=1,
                                                 cpu_fraction=2.0,
                                                 memory="2Gi",
                                                 variation_name="B")
    )
)
```

<Callout icon="📘" theme="info">
  **Note**

  _**Scheduler Timezone**_

  The default timezone for the cron scheduler is UTC.
</Callout>

### Selecting an Instance Type

You can specify the `purchase_option` parameter of the `BuildSpecifications` to select between on-demand and spot instances. Available values: `spot` and `ondemand`. For example:

```
... 
build_spec=BuildSpecifications(
  git_uri="https://github.com/org_id/repository_name.git#dir_1/dir_2",
  git_access_token_secret="token_secret_name",
  git_branch="main",
  main_dir="main",
  tags=["prod"],
  env_vars=["key1=val1", "key2=val2", "key3=val3"],
  purchase_option="ondemand"
),
 ...
```

`spot` is the default value if you don't specify anything.

## Triggering Automations

Automations may be configured to either compare the model's performance with a pre-defined threshold and deploy the model when the evaluation results pass.

We can trigger the automation in two ways:

### Schedule-based Triggers

Triggering an automation based on an interval name or a cron expression.

In the case of an interval configuration, the trigger configuration would look like this:

```
from frogml.core.automations import ScheduledTrigger

# Valid values: Daily, Weekly, Hourly
ScheduledTrigger(interval="Daily")
```

### Metric-based Triggers

To retrain the model based on production performance metrics, we can use the `MetricBasedTrigger`.

In this case, we need to specify a SQL query which should return the metric value from JFrog ML model Analytics:

```
from frogml.core.automations import MetricBasedTrigger, ThresholdDirection, SqlMetric

MetricBasedTrigger(
    name='metric_name',
    metric=SqlMetric(sql_query='SQL_QUERY'),
    direction=ThresholdDirection.ABOVE,
    threshold="0.7"
)
```

## Notifications

JFrog ML supports configuring notifications in case of an error or success using either <Anchor label="Slack Webhooks" target="_blank" href="https://api.slack.com/messaging/webhooks">Slack Webhooks</Anchor> or a custom webhook configuration.

### Slack Webhook

```
from frogml.core.automations import SlackNotification, ScheduledTrigger,FrogmlBuildDeploy,\
    BuildSpecifications, BuildMetric, DeploymentSpecifications, Automation

test_automation = Automation(
    name="my_automation",
    model_id="my_model",
    trigger=ScheduledTrigger(cron="0 0 * * 0"),
    action=FrogmlBuildDeploy(
        build_spec=BuildSpecifications(...),
        deployment_condition=BuildMetric(...),
        deployment_spec=DeploymentSpecifications(...)
    ),
    on_error=SlackNotification(webhook="https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX"),
    on_success=SlackNotification(webhook="https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX")
)
```

It will send a slack message when the automation is finished - containing the execution time, the model, the automation name, and the final status of the automation.

<Callout icon="📘" theme="info">
  **Note**

  You can define alerting notifications for either one of the `on_error` and `on_success` triggers, or for both. `on_success` will trigger after a successful Build, if the deployment threshold was not met and after a successful Build and Deploy, if the threshold was met.
</Callout>

### Custom Webhook

Another option is to be notified to a custom webhook, using the following configuration:

```
from frogml.core.automations import CustomWebhook

on_success=CustomWebhook(
    url="http://my.api/endpoint/v1",
    http_method="GET", # defaults to "GET"
    data={"a1": "b1", "a2": "b2"},
    headers={"content-type": "application/json"}
)
```

The definition above will call the API (defined by the url) above with the following parameters:

* Execution time (`execution_date`)
* Model ID (`model_id`)
* Automation name (`automation`)
* Execution Status (`status`)
* Build ID - (`build_id`)

In addition to the fields explicitly defined in the `data` field.

If the HTTP method defined is `GET` - the `data` field plus the JFrog ML parameters above will be embedded as request parameters. In all other methods - in the body to attached to the request.

## Registering the Automation

Now, we can register the automation using the JFrog ML CLI:

```
frogml automations register -p .
```

In the command above, we specified the directory containing the automation definitions (`-p`). In this case, the current working directory.

## Automating Build and Deploy

Automating model build and deployment helps maintaining models accurate in production.

This action streamlines the build and deployment workflows. It keeps your model accurate by automatically re-training and deploying based on a cron expression, defined time interval, or metric base triggers.

You can also define a deployment conditions to verify that the new build passes acceptance criteria within the desired parameters before replacing a currently deployed model.

#### Automation Example

<Callout icon="⚠️" theme="warning">
  **Warning**

  **Before Setting Up Automation**

  Prior to configuring automation, it's essential to have your model's code stored in a Git repository. It's recommended to confirm that all necessary Git repository access is correctly configured via CLI model builds. Ensure the JFrog ML model can successfully build from Git before proceeding with automation.

  For additional details on building models from Git, refer to our <Anchor label="Build Configurations" title="Build Configurations" href="/docs/build-configurations">Build Configurations</Anchor> page.
</Callout>

The automation will fetch the model's code during the training process. In the case of using a private repository, it is necessary to generate a Git access token and securely store the key in the <Anchor label="Secret Manager" title="Secret Management" href="/docs/secret-management">Secret Manager</Anchor>.

```
from frogml.core.automations import Automation, ScheduledTrigger, FrogmlBuildDeploy,\
    BuildSpecifications, BuildMetric, ThresholdDirection, DeploymentSpecifications

test_automation = Automation(
    name="retrain_my_model",
    model_id="my-model-id",
    trigger=ScheduledTrigger(cron="0 0 * * 0"),
    action=FrogmlBuildDeploy(
        build_spec=BuildSpecifications(git_uri="https://github.com/org_id/repository_name.git#dir_1/dir_2",
                                       git_access_token_secret="token_secret_name",
                                       git_branch="main",
                                       main_dir="main",
                                       tags=["prod"],
                                       env_vars=["key1=val1", "key2=val2", "key3=val3"]),
        deployment_condition=BuildMetric(metric_name="f1_score",
                                         direction=ThresholdDirection.ABOVE,
                                         threshold="0.65"),
        deployment_spec=DeploymentSpecifications(number_of_pods=1,
                                                 cpu_fraction=2.0,
                                                 memory="2Gi",
                                                 variation_name="B")
    )
)
```

<Callout icon="📘" theme="info">
  **Note**

  _**Scheduler Timezone**_

  The default timezone for the cron scheduler is UTC.
</Callout>

#### Build & Deploy Configuration

The `FrogmlBuildDeploy` action has three configuration parameters:

1. `build_spec` defines the location of the model code that we will build in the JFrog ML platform.
2. `deployment_condition` defines the metrics used to determine when to deploy the model after the training.
3. `deployment_spec` specifies the runtime environment parameters for model deployment.

<Callout icon="⚠️" theme="warning">
  **Warning**

  Metrics used to trigger build or deploy automations must be logged during the model build phase.
</Callout>

##### `BuildSpecifications`

To configure the automation build specification, we need a link to the git repository.

Note that the link consists of two parts delimited by hashtag `#`:

* The repository URL
* The path within the repository

For example, when we use this link: `https://github.com/org_id/repository_name.git#dir_1/dir2` .

The platform will clone the `https://github.com/org_id/repository_name.git` repository and change the working directory to `dir_1/dir_2` before starting the build.

In this example, `dir_1/dir_2` should be the directory containing the `main` and `tests` folders.

##### Using Private Repositories

When using private repositories, we must also specify the access token or private key.

As the JFrog ML platform doesn't allow the usage of plain text token, we must store the access tokens in the [JFrog ML Secret Manager](/docs/doc:secret-management#model-build-credentials), and specify only the secret name.

When not using the default folder structure, in which `main` is the models folder, we must also specify the git branch and the directory containing the ML model.

##### Custom Resources

In the build specification, you may control the number of CPUs, amount of memory or use GPUs [Instance Sizes](/docs/instance-sizes-ml-credits)

**Defining CPU resources:**

```
resources=CpuResources(cpu_fraction=2, memory="2Gi"))
```

**Defining GPU resources:**

```
resources=GpuResources(gpu_type="NVIDIA_K80", gpu_amount=1)
```

Alternatively, you can specify the instance type as opposed to fractions of resources. For example:

```
resources=ClientResources(instance='gpu.a10.8xl') #GPU

#OR

resources=ClientResources(instance='medium')  #CPU
```

It is possible specify the IAM role used in production (`assumed_iam_role`) or a custom docker image (`base_image`).

#### Environment Variables

Additionally, we can specify the environment variables to configure in the build environment.

he environment variables should be specified with the _env_vars_ field (list), and the value as the following:

`key=value`.

The model's code must log the metric that describes the model's performance. We will use the metric in the deployment condition. If you don't know how to do it, look at our Logging and Monitoring Guide.

#### Disable Push Image

It is possible to disable the push image phase in cases you don't want the final build saved to the docker repository. You can do that by adding `push_image=False` to the BuildSpecification

###### `BuildMetric`

During the build process, it is common to log metrics such as accuracy, F1 score, or loss. When executing the automation, these logged values may be compared against a specified threshold.

For each metric, it is possible to define whether the value should be above or below the threshold. Once this condition is met, the JFrog ML platform will proceed to deploy the model.

The `BuildMetric` object has three parameters:

1. **metric_name**: The metric name we logged during the build phase
2. **direction**: Show the value be below or above the threshold, where the valid values are `ThresholdDirection.ABOVE`, `ThresholdDirection.BELOW`
3. **threshold**: The threshold used for comparison

<Callout icon="⚠️" theme="warning">
  **Warning**

  The threshold must always be a string, where `threshold="0.65"` is a valid threshold and `threshold=0.65` is invalid!
</Callout>

##### Dynamic Threshold

To use a dynamic threshold, we can use a SQL expression as the threshold value.

In this case, the JFrog ML platform will run the SQL query in JFrog ML Model Analytics and compare the model's metric with the threshold produced by the SQL query.

The query must return a single row containing only one column.

##### `DeploymentSpecifications`

After we build the model, compared its performance with the threshold, and concluded that the model is ready to be deployed, the platform will use the deployment specification to configure the model's runtime environment.

We may specify:

<Table>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Details
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        number_of_http_server_workers
      </td>

      <td>
        The number of threads used by the HTTP server.
      </td>
    </tr>

    <tr>
      <td>
        http_request_timeout_ms
      </td>

      <td>
        The request timeout.
      </td>
    </tr>

    <tr>
      <td>
        daemon_mode
      </td>

      <td>
        Should gunicorn process be daemonized, which makes the workers work in the background.
      </td>
    </tr>

    <tr>
      <td>
        custom_iam_role_arn
      </td>

      <td>
        The IAM role used in production.
      </td>
    </tr>

    <tr>
      <td>
        max_batch_size
      </td>

      <td>
        Max batch size of record.
      </td>
    </tr>

    <tr>
      <td>
        deployment_process_timeout_limit
      </td>

      <td>
        The timeout for the deployment (in seconds).
      </td>
    </tr>

    <tr>
      <td>
        number_of_pods
      </td>

      <td>
        The number of instances to be deployed.
      </td>
    </tr>

    <tr>
      <td>
        cpu_fraction
      </td>

      <td>
        The CPU cores for Kubernetes.
      </td>
    </tr>

    <tr>
      <td>
        memory
      </td>

      <td>
        The amount of RAM.
      </td>
    </tr>

    <tr>
      <td>
        variation_name
      </td>

      <td>
        The variant name if we run an A/B test.
      </td>
    </tr>

    <tr>
      <td>
        auto_scale_config
      </td>

      <td>
        The autoscaling configuration for Kubernetes.
      </td>
    </tr>

    <tr>
      <td>
        min_replica_count
      </td>

      <td>
        The minimum number of replicas the resource will be scaled down to.
      </td>
    </tr>

    <tr>
      <td>
        max_replica_count
      </td>

      <td>
        The maximum number of replicas of the target resource.
      </td>
    </tr>

    <tr>
      <td>
        polling_interval
      </td>

      <td>
        This is the interval for which to check each trigger. By default, it's every 30 seconds.
      </td>
    </tr>

    <tr>
      <td>
        cool_down_period
      </td>

      <td>
        The period to wait after the last trigger reported active before scaling the resource back to 0. By default it's 5 minutes (300 seconds).
      </td>
    </tr>

    <tr>
      <td>
        prometheus_trigger
      </td>

      <td>
        metric_type: The type of the metric - cpu/gpu/memory/latency

        aggregation_type: The type of the aggregation - min/max/avg/sum

        time_period: The period to run the query based on

        threshold: Value to start scaling for
      </td>
    </tr>

    <tr>
      <td>
        Environments
      </td>

      <td>
        List of environment names to deploy to.
      </td>
    </tr>
  </tbody>
</Table>

#### Defining Auto-Scaling

When we want to define an auto-scaling policy for our deployment, we have to use the following pattern:

```
auto_scale_config = AutoScalingConfig(min_replica_count=1,
                                          max_replica_count=10,
                                          polling_interval=30,
                                          cool_down_period=300,
                                          triggers=[
                                              AutoScalingPrometheusTrigger(
                                                  query_spec=AutoScaleQuerySpec(
                                                      aggregation_type="max",
                                                      metric_type="latency",
                                                      time_period=4),
                                                  threshold=60
                                              )
                                          ]
                                          )
```

## Automating Batch Execution

Automating batch model inference simplifies running batch tasks on a periodic basis..

You can set up scheduled executions to dynamically process data and ensure regular updates without manual intervention, or a need to setup external scheduling tools such as Airflow.

###### Configuring `BatchExecution`

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

#### Dynamic Folder Paths

When configuring the source and destination folders to read and write data, we may define folder paths based on the runtime timestamp.

The dynamic path may include a timestamp template, which will be injected when the automation runs.

<Callout icon="📘" theme="info">
  **Note**

  he timestamp format should follow <Anchor label="**Python strftime**" target="_blank" href="https://strftime.org/">**Python strftime**</Anchor> formatting, and wrapped with curly brackets, i.e.: `{%d-%m-%Y}`
</Callout>

#### Example Path Template

There are two input parameters which support the dynamic timestamp template:

* `destination_folder`
* `source_folder`

Defining `source_folder="input_folder/{%d-%m-%Y}"` will format the path based on the current timestamp:

`source_folder="input_folder/23-05-2023"`
