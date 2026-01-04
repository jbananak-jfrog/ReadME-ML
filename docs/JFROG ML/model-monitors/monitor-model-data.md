---
title: Monitor Model Data
deprecated: false
hidden: false
metadata:
  title: Monitor Model Data
  description: >-
    Monitoring model data plays a pivotal role in the lifecycle of machine
    learning models, particularly when deploying models in production
    environments.
  legacyUUIDs:
    - UUID-45531d4c-ebf1-6cb0-500d-1118947a9d8e
    - UUID-580d3dd0-992d-ca72-811e-a640c21006e1
  robots: index
---
Monitoring model data plays a pivotal role in the lifecycle of machine learning models, particularly when deploying models in production environments.

JFrog ML model monitoring helps you to track selected model inputs and outputs, while enabling automatic alerts for detecting data inconsistencies.

<Callout icon="📘" theme="info">
  **Note** - _**Beta Feature**_

  Model monitoring is currently in beta phase, and is being actively developed and refined. JFrog is continuously working to improve features and performance based on valuable user feedback.
</Callout>

<Image alt="Model Monitoring Overview" border={false} src="https://files.readme.io/ed137ab2d64086c63e66de2fa84a2c87563f4ede2d3d8e3473f6154ad6a5ae04-uuid-e4d616bc-fdd0-ad78-bd7b-580f79703e47.png" />

### Monitoring KL Divergence

<Callout icon="❗️" theme="error">
  **Important**

  Please <Anchor label="configure Slack channels" target="_blank" href="https://github.com/qwak-ai/sdk-examples/blob/main/model-monitoring/add_alert_channels.py">configure Slack channels</Anchor> to which alerts are sent before setting up new monitors.
</Callout>

KL divergence measures the difference between the probability distributions of the model predictions and the actual outcomes, using the <Anchor label="Kullback-Leibler (KL) divergence" target="_blank" href="https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence">Kullback-Leibler (KL) divergence</Anchor>. This helps detect data discrepancies and assess model performance over time.

### Create KL Divergence Monitors

Creating a new model monitor can be done in several simple steps:

1. **Select the Model:** Select the specific model that you want to monitor.
2. **Navigate to Monitors:** Open the **Monitors** tab in your model, where monitors are managed.
3. **Create a New Monitor:** Create a new monitor by clicking **Create Monitor**.
4. **Choose Calculation Method:** Select the KL Divergence calculation function.
5. **Select Monitoring Dimension:** Indicate the dimension you wish to monitor, either a model input or output.
6. **Define Baseline:** Set up a baseline for the calculation, which serves as a reference point to the KL divergence calculation. See below for more details.
7. **Preview Monitor Data:** Click **Preview** to see the monitor data before defining the alert threshold.
8. **Set Alert Parameters:** Specify the criteria for triggering alerts. Define the threshold and conditions for triggering a monitoring alert.
9. **Configure Alert Channels:** Decide which Slack channels will received the monitoring alerts.
   ![](https://files.readme.io/d9841e9b4dc3de72988d18a89a61523093e9990a7e8625e77e1c3a72a8da3a27-uuid-aa2f0546-b478-1178-53d2-65776e1c24a1.png)

### Build-based Baselines

* When choosing a build-based baseline, you can choose the training data in one of the previous model builds as a reference point.
* When choosing a build-based baseline, the logged data set from that build will be using for KL divergence calculation.
* The rocket ship icon indicates the currently deploy build version.

<Callout icon="❗️" theme="error">
  **Important**

  _**Tagging Training Data**_

  Using build-based baseline requires a logging a reference training data during the model build.

  Tagging data sets is done by using: `frogml.log_data(dataframe=df, tag="train_data")` for example.

  KL divergence calculation cannot use builds without logged data sets.
</Callout>

<Image align="center" alt="Selecting a build-based baseline" border={false} width="70px" src="https://files.readme.io/286def2188cf95ac0cbdba96dee4f1cd2c46766fc555f48226a789da62b20dad-uuid-cf1570c6-b4a9-8e11-91aa-6c9bf5f2f9e9.png" />

### Using Static Baseline

A static baseline defines the timeframe with which KL divergence data will be calculated. The system takes as reference all the received values during this time period and will calculate KL divergence against it.

<Image align="center" border={false} src="https://files.readme.io/0eab00ac48850186326fe2256061dcc7e24c4e9374689519c54fa15524cf8081-usingstaticbaseline.png" />

### Using Sliding Window Baseline

Sliding window is measured in minutes, and specifies the length of the sliding window used for computing KL divergence.

The default sliding window size is 60 minutes.

<Image align="center" border={false} src="https://files.readme.io/da2d65b379330c21687f920f60d112aa1c3a6360185509f6cadf6fd95e399113-usingslidingwindowbaseline.png" />

### Monitoring Null Percentage

Monitoring null percentage refers to tracking and analyzing the proportion of missing or null values in a dataset or a particular feature within a dataset.

Missing values are common, and monitoring their occurrence can be crucial for ensuring the quality and reliability of your models.

Configuring null percentage monitors is simpler compared to KL divergence monitors, as the only required parameter is the dimension you wish to monitor.

<Image align="center" border={false} src="https://files.readme.io/229a1fb7f0c057c2f13e934ea3ce30ee7cf596356202d208f73c45ab2fbbeb77-monitoringnullpercentage.png" />

### Configuring Monitor Parameters

Under the _Advanced Configuration_ menu are some additional parameters you may configure:

1. **Monitor Name:** Modify the default monitor name (monitor names are unique).
2. **Frequency (8 hours):** The time interval between sample points within the entire monitoring period.
3. **Evaluation Window (60 min):** The time window to look back at every monitoring sample point.

### Configuring Alerts

<Callout icon="📘" theme="info">
  **Note** - _**Channels Integrations**_

  Prior to setting up alerts, please make sure that the relevant integrations and channels are configured. For more information, please see the [Alert Integrations](/docs/jfrog-ml-alerts) guide.
</Callout>

Alerts help you stay up-to-date with your model in real time. After configuring your model monitor details, choose the adequate threshold and alerting condition, whether above or below the threshold value.

<Image align="center" border={false} src="https://files.readme.io/870f9e78b7412dccd756db2d5bed0649f5da5d8f31c61133869f376c3d504154-configuringalerts.png" />

<Image align="center" border={false} src="https://files.readme.io/84a508294fab6274e0e20c5f2b39f1dac158fcc7bc33ed249e6ac771cb4b7a06-configuringalerts.png" />

### Tags and Priority

Tags and priority help you organize model monitor alerts. Tags let you categorize alerts, while priority helps in sorting alerts as they occur.

<Image alt="Configuring tags and priority for alerts" border={false} src="https://files.readme.io/abf39544e7a2a9183c7dac3a7159542e84e0e690d203867f323b245c78c11942-uuid-12133894-5bcf-191d-fa4e-db5423db87cd.png" />

### Priority

You can assign an alert priority to your channel. Each priority level will be mapped appropriately to the corresponding integration. Please note that the priority mapping to the different integrations is defined on the platform level.

| JFrog ML           | Opsgenie | Pagerduty | Slack    |
| :----------------- | :------- | :-------- | :------- |
| Critical           | P1       | critical  | Critical |
| High               | P2       | error     | High     |
| Moderate (default) | P3       | warning   | Moderate |
| Low                | P4       | info      | Low      |
| Info               | P5       | info      | Info     |

### Tags

You can define up to 20 text tags for your Opsgenie alerts, with each tag having a maximum length of 50 characters.

<Callout icon="📘" theme="info">
  **Note**

  Tags are supported **only** for Opsgenie channels.
</Callout>
