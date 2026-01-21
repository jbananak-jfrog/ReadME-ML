---
title: Define Automations
excerpt: >-
  JFrog ML offers built-in automations for various recurring actions in the
  model lifecycle. These automations are meant to replace external orchestration
  tools such as Airflow for example.
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
<br />

In the JFrog ML platform, you can easily define either time-based or trigger-based automations for model retraining or batch model executions.

## Configure Automations

<Callout icon="⚠️" theme="warning">
  **Warning**

  The automation name must be unique throughout your models within the JFrog ML environment.
</Callout>

To configure an automation, create an instance of the Automation class and configure it:

```python
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
  **Note** - _**Scheduler Timezone**_

  The default timezone for the cron scheduler is UTC.
</Callout>

### Select an Instance Type

You can specify the `purchase_option` parameter of the `BuildSpecifications` to select between on-demand and spot instances. Available values: `spot` and `ondemand`. For example:

```python
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

## How to Triggering Automations

Automations can be configured to either compare the model's performance with a pre-defined threshold and deploy the model when the evaluation results pass. The automations can be triggered in two ways:

### Schedule-based Triggers

Automations are triggered based on an interval name or a cron expression.

In the case of an interval configuration, the trigger configuration would look like this:

```
from frogml.core.automations import ScheduledTrigger

# Valid values: Daily, Weekly, Hourly
ScheduledTrigger(interval="Daily")
```

### Metric-based Triggers

To retrain the model based on production performance metrics, use the `MetricBasedTrigger`.

In this case, specify a SQL query which should return the metric value from JFrog ML model Analytics:

```python
from frogml.core.automations import MetricBasedTrigger, ThresholdDirection, SqlMetric

MetricBasedTrigger(
    name='metric_name',
    metric=SqlMetric(sql_query='SQL_QUERY'),
    direction=ThresholdDirection.ABOVE,
    threshold="0.7"
)
```

## Notifications

JFrog ML supports configuring notifications in case of an error or success using either <Anchor label="Slack Webhooks" target="_blank" href="https://api.slack.com/messaging/webhooks">Slack Webhooks</Anchor> or a [custom webhook](/docs/define-automations#custom-webhook) configuration.

### Slack Webhook

This option sends a Slack message when the automation is finished - containing the execution time, the model, the automation name, and the final status of the automation.

```python
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
    on_error=SlackNotification(webhook="https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX"), on_success=SlackNotification(webhook="https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX")
)
```

***

<Callout icon="📘" theme="info">
  **Note**

  You can define alerting notifications for either one of the `on_error` and `on_success` triggers, or for both. `on_success` will trigger after a successful build if the deployment threshold was not met, and after a successful Build and Deploy if the threshold was met.
</Callout>

### Custom Webhook

The alternative notification option is to be notified to a custom webhook, using the following configuration:

```python
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

If the HTTP method defined is `GET` - the `data` field plus the JFrog ML parameters above will be embedded as request parameters. In all other methods, in the body to attached to the request.

## Register the Automation

Once configured and notification is set up, register the automation using the JFrog ML CLI:

```shell
frogml automations register -p .
```

In the command above, the directory containing the automation definitions (`-p`) is specified; in this case, the current working directory.

<br />
