---
title: Performance Monitors
deprecated: false
hidden: false
metadata:
  title: Performance Monitors
  description: Monitoring ML model performance on JFrog ML is essential for maintaining the reliability and effectiveness of your models. Our integrated alerting system enables you to effortlessly track key metrics and receive realtime notifications. It seamlessly integrates with your preferred communication and incident management tools, such as Slack, OpsGenie or PagerDuty.
  robots: index
  legacyUUIDs:
    - UUID-b5e59423-b775-d6f3-185b-e85a03fdc88a
    - UUID-613d4616-9c5e-92d1-582b-161a455cbb58
---
Monitoring ML model performance on JFrog ML is essential for maintaining the reliability and effectiveness of your models. Our integrated alerting system enables you to effortlessly track key metrics and receive realtime notifications. It seamlessly integrates with your preferred communication and incident management tools, such as Slack, OpsGenie or PagerDuty.

### Configuring Infrastructure Monitors

![Infrastructure Monitor Configuration Screen](../image/uuid-0c2c7ade-1d96-3e88-c3e2-6893ae7e7292.png)

1. Open the model you'd like to monitor and switch to the **Monitors** tab.
2. Click **Create New Monitor** -> **Infrastructure Monitor**.
3. In the **Type** field, choose the desired metric to alert based on:

   * ***Error Rate*** refers to how many requests returned an error in the given interval (Duration)
   * ***Throughput*** refers to the amount of requests per given interval, this is great for a use case where you'd like to be alerted when a scaling policy should kick in.
   * ***Latency*** 95, 90 and 50 signifies the slowest 5, 10 and 50% of requests (highest latency) from all the requests received in the given interval.
4. In the **Aggregation** you can select what aggregation is relevant for the monitoring metric.
5. **Variation** is the model version that you'd like to get alerted on. Generally Default when the model is deployed under one variation only.
6. Under the **Alerting** tab you will find the Condition, Threshold and the Duration which is the aggregation interval.
7. ![create-new-monitor.png](../image/uuid-f563a35d-660e-8ddb-4cce-97c7c6c5bdba.png)
8. From the **Channels** dropdown, select a channel to receive the notifications. If you don't see your channel there, follow the instructions below to add a new channel.
9. Pick which model variant should be tracked or choose "All variations".
10. Remember to save your enable the alert by clicking the **Status** toggle and save the changes using the **Save** button!
11. For deployments of type **Streaming** the only supported alert types are "Error rate" and "Throughput". All alerts of type "Latency" will be automatically disabled. Remember to re-enable those alerts in case you change your deployment time to **Realtime**

### Tags and Priority

![tags-and-priorities.png](../image/uuid-12133894-5bcf-191d-fa4e-db5423db87cd.png)

### Priority

You can assign an alert priority to your channel. Each priority level will be mapped appropriately to the corresponding integration.



<Table>
  <thead>
    <tr>
      <th>
        Jfrog ML
      </th>
      <th>
        Opsgenie
      </th>
      <th>
        Pagerduty
      </th>
      <th>
        Slack
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        Critical
      </td>
      <td>
        P1
      </td>
      <td>
        critical
      </td>
      <td>
        Critical
      </td>
    </tr>
    <tr>
      <td>
        High
      </td>
      <td>
        P2
      </td>
      <td>
        error
      </td>
      <td>
        High
      </td>
    </tr>
    <tr>
      <td>
        Moderate
      </td>
      <td>
        P3
      </td>
      <td>
        warning
      </td>
      <td>
        Moderate
      </td>
    </tr>
    <tr>
      <td>
        Low
      </td>
      <td>
        P4
      </td>
      <td>
        info
      </td>
      <td>
        Low
      </td>
    </tr>
    <tr>
      <td>
        Info
      </td>
      <td>
        P5
      </td>
      <td>
        info
      </td>
      <td>
        Info
      </td>
    </tr>
  </tbody>
</Table>



### Tags

You can define up to 20 text tags for your Opsgenie alerts, with each tag having a maximum length of 50 characters.

<Callout icon="❗️" theme="error">
**Important**

Can **only** be used for the Opsgenie integration.
</Callout>


### Configuring Channels

For channels integrations you can follow the next instruction: [JFrog ML Alerts](/docs/jfrog-ml-alerts "JFrog ML Alerts").
