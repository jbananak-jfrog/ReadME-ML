---
title: Automation Tutorials
deprecated: false
hidden: false
metadata:
  title: Automation Tutorials
  description: 'This section reviews the following topics:'
  robots: index
  legacyUUIDs:
  - UUID-2b7e111b-a22a-ed97-c020-22f89890b44e
  - UUID-843d1909-0802-5c3c-b8bb-6490d6e89739
  - UUID-292e25f5-4adb-fd5b-5df5-83ee263cdea0
  - UUID-675c1c56-8cc0-b0f9-6b7f-102c5ed27d3a
  - UUID-6dc2106c-0b63-227f-de25-f28fd01f9b8f
  - UUID-1cf01102-eee8-2e84-6770-93da84a68ad4
---

This section reviews the following topics:

[Scheduled Training and Deployment](/docs/scheduled-training-and-deployment "Scheduled Training and Deployment")

[Monitoring Batch Execution Failures](/docs/monitoring-batch-execution-failures "Monitoring Batch Execution Failures")

## Scheduled Training and Deployment

To retrain an ML model and deploy the new version periodically, we'll need a schedule-based automation.

In this tutorial, we show how to configure the scheduled automation.

When does it make sense to use the scheduled-based automation?

It won't help to retrain the model when using the same training data every time.

Because of that, the model's `build` function should retrieve the up-to-date training data from <Anchor label="the Feature Store" href="https://jfrog.com/blog/what-is-a-feature-store-in-ml-and-do-i-need-one/" target="_blank">the Feature Store</Anchor>.

If you aren't familiar with the JFrog ML Feature Store, check out our [QuickStart](/docs/feature-store-quick-start-guide "Feature Store Quick Start Guide") guide.

#### Pre-requisites

Make sure you store the model code in a Git. We will need the repository URL and the access token later on.

#### Configuration

First, we create an empty Python script and define the import the dependencies and create an instance of the Automation class and configure it:

```
from frogml.core.automations  import Automation, ScheduledTrigger, \
      FrogmlBuildDeploy,BuildSpecifications, BuildMetric, \
        ThresholdDirection, DeploymentSpecifications

test_automation = Automation(
    name="automation_name",
    model_id="model_to_be_deployed",
    trigger=ScheduledTrigger(cron="0 0 * * 0"),
    action=FrogmlBuildDeploy(
        build_spec=BuildSpecifications(git_uri="https://github.com/org_id/repository_name.git#directory/another_directory",
                                       git_access_token_secret="secret_name",
                                       git_branch="main",
                                       main_dir="main",
                                       tags=["prod"],
                                       env_vars=["key1=val1","key2=val2","key3=val3"]),
        deployment_condition=BuildMetric(metric_name="f1_score",
                                         direction=ThresholdDirection.ABOVE,
                                         threshold="0.65"),
        deployment_spec=DeploymentSpecifications(number_of_pods=1,
                                                 cpu_fraction=2.0,
                                                 memory="2Gi",
                                                 variation_name="B",
                                                 environments=["env1","env2"])
    )
)
```

We have described the configuration parameters in our [Automating Build and Deploy](/docs/automating-build-and-deploy "Automating Build and Deploy") page.

#### Publishing the Automation

Finally, we can use the JFrog ML CLI to publish the automation. We specify the name of the JFrog ML environment `--environment` and the directory containing the automation definitions`-p`,  In this case, the current working directory.

```
frogml automations register --environment environment_name -p .
```

## Monitoring Batch Execution Failures

This documentation should guide you through setting up a monitoring solution that checks for failed batch executions on JFrog ML and sends notifications to Slack using an AWS Lambda. Feel free to adjust the scripts as needed to fit your specific infrastructure and notification requirements.

#### Requirements:

To set up this batch monitoring automation, you'll need the following:

* **Cron job infrastructure**: We will use AWS Lambda to schedule and run the script periodically.
* **Notification platform**: We will use Slack to receive notifications about any batch job failures.

#### 1. Check the batch executions that finished in the last N minutes/hours

We need a Python script to checks for batch executions that have finished within a specified time window and identifies any failures. This helps us monitor the status of our jobs and react promptly to any issues.

```
# check_and_retrieve_failed_executions.py
import os
from frogml import FrogMLClient
from frogml.sdk.frogml_client.batch_jobs.execution import Execution
from datetime import datetime, timedelta


def check_and_retrieve_failed_executions(minutes_to_search_back):
  
    # Initialize the FrogML client
    client = FrogMLClient()
    failed_executions = []

    # Set the time window to check for executions
    time_threshold = datetime.now() - timedelta(minutes=minutes_to_search_back)

    # List all projects
    projects = client.list_projects()

    # Iterate through projects and their models to check executions
    for project in projects:
        models = client.list_models(project.project_id)
        for model in models:
            executions = client.list_executions(model=model.model_id)
            for execution in executions:
                # Check if the execution failed within the last N minutes
                if execution.end_time >= time_threshold and execution.execution_status == ExecutionStatus.BATCH_JOB_FAILED_STATUS:
                    failed_executions.append({
                        'execution_id': execution.execution_id,
                        'model_id': model.model_id,
                        'failure_message': execution.failure_message
                    })

    # Create a message with failed executions
    if failed_executions:
        message = "Failed Executions in the Last {} Minutes:\n".format(minutes_to_search_back)
        for failure in failed_executions:
            message += "Execution ID: {}\nModel ID: {}\nFailure Message: {}\n\n".format(
                failure['execution_id'], failure['model_id'], failure['failure_message']
            )
        return message
    else:
        return None
```

#### 2. Sending a notification to Slack with all the failed alerts for the last N minutes

```
# notify_on_slack.py
import requests
import json

def send_message_to_slack(message, webhook_url):
    payload = {"text": message}
    
    headers = {'Content-Type': 'application/json'}
    
    response = requests.post(webhook_url, data=json.dumps(payload), headers=headers)
    
    if response.status_code != 200:
        raise ValueError(f'Request to Slack returned an error {response.status_code}, the response is: {response.text}')
    else:
        print('Message posted successfully.')
```

#### 3. Calling everything in the Lambda Handler

```
# lambda_handler.py
import os
from check_and_retrieve_failed_executions import check_and_retrieve_failed_executions
from notify_on_slack import send_message_to_slack

def monitor_executions(event, context):
    # Retrieve environment variables
    jfrog_api_key = os.environ.get('JFROG_API_KEY')
    webhook_url = os.environ.get('SLACK_WEBHOOK_URL') ### 'https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX'
    minutes_to_search_back = int(os.environ.get('MINUTES_TO_SEARCH_BACK', 60))

    # Check for missing environment variables
    if not jfrog_api_key or not webhook_url:
        raise EnvironmentError("Please set the 'JFROG_API_KEY' and 'SLACK_WEBHOOK_URL' environment variables."
    
    failed_executions_message = check_and_retrieve_failed_executions(minutes_to_search_back)
    
    if "No failed executions found" not in failed_executions_message:
        send_message_to_slack(failed_executions_message, webhook_url)
```

#### Putting It All Together

Steps to Deploy the automation on AWS Lambda:

1. **Create a new Lambda function** in the AWS Lambda console.
2. **Set the runtime** to Python 3.x.
3. **Add environment variables** in the Lambda function configuration for:

   * `JFROG_API_KEY`: Your JFrog API key.
   * `SLACK_WEBHOOK_URL`: Your Slack webhook URL.
   * `MINUTES_TO_SEARCH_BACK`: The number of minutes to check back for failed executions. Defaults to 60 if not set.
4. **Upload the three Python scripts** (`check_and_retrieve_failed_executions.py`, `notify_on_slack.py`, `lambda_handler.py`) as a Lambda deployment package (zip file).
5. **Configure the function handler** in the Lambda console to `lambda_handler.monitor_executions`.
6. **Set up a CloudWatch event rule** to trigger the Lambda function periodically according to your preferred schedule (e.g., every hour).

By following these steps, you will be able to monitor JFrog ML batch executions for failures and receive alerts via Slack, enriching the functionality already existent on the platform.
