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

<Image align="left" alt="Selecting a static baseline" border={false} width="50% " src="https://files.readme.io/1c8e19a7187f8482fd217c0113e754827b1201f2bdca616bcb5c5ba2b2dbb0ca-uuid-fdab6159-0b57-6856-8e4f-df1f93d809e0.png" />

### Using Sliding Window Baseline

Sliding window is measured in minutes, and specifies the length of the sliding window used for computing KL divergence.

The default sliding window size is 60 minutes.

<Image alt="Selecting a sliding window baseline" border={false} src="https://files.readme.io/8881991d286109f8ebabcdc4b82866f7a4e1dcb2b344694fce4ae271c2f33711-uuid-2f47b69a-9e60-2373-c5f3-136fead37d04.png" />

### Monitoring Null Percentage

Monitoring null percentage refers to tracking and analyzing the proportion of missing or null values in a dataset or a particular feature within a dataset.

Missing values are common, and monitoring their occurrence can be crucial for ensuring the quality and reliability of your models.

Configuring null percentage monitors is simpler compared to KL divergence monitors, as the only required parameter is the dimension you wish to monitor.

<Image alt="Configuring a null percentage monitor" border={false} src="https://files.readme.io/4ef620b585bd5780496ed912eb44ca1f79e14b3c009c175dec8dd5754d2872de-uuid-2032bd06-a23f-c76c-1014-5e324c3a5bfe.png" />

### Configuring Monitor Parameters

Under the _Advanced Configuration_ menu are some additional parameters you may configure:

1. **Monitor Name:** Modify the default monitor name (monitor names are unique).
2. **Frequency (8 hours):** The time interval between sample points within the entire monitoring period.
3. **Evaluation Window (60 min):** The time window to look back at every monitoring sample point.

### Configuring Alerts

<Callout icon="📘" theme="info">
  **Note**

  _**Channels Integrations**_

  Prior to setting up alerts, please make sure that the relevant integrations and channels are configured. For more information, please see the <Anchor label="Alert Integrations" target="_blank" href="https://jfrog.info/help/r/jfrog-ml-documentation/alerts">Alert Integrations</Anchor> guide.
</Callout>

Alerts help you stay up-to-date with your model in real time. After configuring your model monitor details, choose the adequate threshold and alerting condition, whether above or below the threshold value.

<Image alt="Configuring alert parameters" border={false} src="https://files.readme.io/f61050e2754ed260053b8e6909425852b11cad4187ed98ca8d49f948f33b9f37-uuid-5d91396d-3eb4-f170-96c5-98a19fa972c3.png" />

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
