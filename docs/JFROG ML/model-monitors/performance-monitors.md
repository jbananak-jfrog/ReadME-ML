---
title: Performance Monitors
excerpt: >-
  Monitoring ML model performance on JFrog ML is essential for maintaining the
  reliability and effectiveness of your models.
deprecated: false
hidden: false
metadata:
  title: Performance Monitors
  description: >-
    Monitoring ML model performance on JFrog ML is essential for maintaining the
    reliability and effectiveness of your models. Our integrated alerting system
    enables you to effortlessly track key metrics and receive realtime
    notifications. It seamlessly integrates with your preferred communication
    and incident management tools, such as Slack, OpsGenie or PagerDuty.
  legacyUUIDs:
    - UUID-b5e59423-b775-d6f3-185b-e85a03fdc88a
    - UUID-613d4616-9c5e-92d1-582b-161a455cbb58
  robots: index
---
Use your JFrog ML integrated alerting system to effortlessly track key metrics and receive realtime notifications. Seamlessly integrate with your preferred communication and incident management tools, such as Slack, OpsGenie or PagerDuty.

### Configure Infrastructure Monitors

<Image alt="Infrastructure Monitor Configuration Screen" border={false} src="https://files.readme.io/1255bebb2b5d220428ede0c8b731fc408bc9b21024983d7b22519037512c2ac1-uuid-0c2c7ade-1d96-3e88-c3e2-6893ae7e7292.png" />

1. Open the model you'd like to monitor and switch to the **Monitors** tab.
2. Click **Create New Monitor** -> **Infrastructure Monitor**.
3. In the **Type** field, choose the desired metric to alert based on:

   * _**Error Rate**_ refers to the number of requests that returned an error in the given interval (Duration)
   * _**Throughput**_ refers to the number of requests per given interval. Use in cases where you would like to be alerted when a scaling policy should kick in.
   * _**Latency**_ 95, 90 and 50 signifies the slowest 5, 10 and 50% of requests (highest latency) from all the requests received in the given interval.
4. In the **Aggregation** you can select what aggregation is relevant for the monitoring metric.
5. **Variation** is the model version that you'd like to get alerted on. Generally Default when the model is deployed under one variation only.
6. Under the **Alerting** tab you will find the Condition, Threshold and the Duration which is the aggregation interval.

   <Image alt="create-new-monitor.png" border={false} src="https://files.readme.io/438db3826dff69b8be373a34a34bc694288d67caea92ade27d52ab3141217eaf-uuid-f563a35d-660e-8ddb-4cce-97c7c6c5bdba.png" />
7. From the **Channels** dropdown, select a channel to receive the notifications. If you don't see your channel there, follow the instructions below to add a new channel.
8. Pick which model variant should be tracked or choose "All variations".
9. Remember to save your enable the alert by clicking the **Status** toggle and save the changes using the **Save** button!
10. For deployments of type **Streaming** the only supported alert types are "Error rate" and "Throughput". All alerts of type "Latency" will be automatically disabled. Remember to re-enable those alerts in case you change your deployment time to **Realtime**

### Tags and Priority

<Image align="center" border={false} src="https://files.readme.io/b553bfd6a07e4b3dfa7e9206ae5b1be4d6e30bdadb35274915898cb9932b808c-tagsandpriority-performancemonitors.png" />

### Priority

You can assign an alert priority to your channel. Each priority level will be mapped appropriately to the corresponding integration.

| Jfrog ML | Opsgenie | Pagerduty | Slack    |
| :------- | :------- | :-------- | :------- |
| Critical | P1       | critical  | Critical |
| High     | P2       | error     | High     |
| Moderate | P3       | warning   | Moderate |
| Low      | P4       | info      | Low      |
| Info     | P5       | info      | Info     |

### Tags

You can define up to 20 text tags for your Opsgenie alerts, with each tag having a maximum length of 50 characters.

<Callout icon="❗️" theme="error">
  **Important**

  Can **only** be used for the Opsgenie integration.
</Callout>

### Configuring Channels

For channels integrations you can follow the next instruction: [JFrog ML Alerts](/docs/jfrog-ml-alerts).

<br />
