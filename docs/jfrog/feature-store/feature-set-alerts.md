---
title: Feature Set Alerts
deprecated: false
hidden: false
metadata:
  title: Feature Set Alerts
  description: Feature set alerts help you track and maintain the status of batch feature sets.
  robots: index
  legacyUUIDs:
    - UUID-c66db99d-a45e-a926-b2f4-ef3b60704065
    - UUID-4c45e900-4f7f-b00a-b5a2-eb89b9b71c83
---
Feature set alerts help you track and maintain the status of batch feature sets.

Currently, only alerts for batch feature set failures are supported. These alerts notify you when a batch feature set ingestion fails, enabling you to take immediate action.

<Callout icon="📘" theme="info">
**Note**

***Monitoring Integrations***

Before setting up alerts, please make sure that the relevant integrations and channels are configured. For more information, please see the [Alert Integrations](/docs/jfrog-ml-alerts "JFrog ML Alerts") guide.
</Callout>


### Creating Feature Set Alerts

1. Navigate to <Anchor label="JFrog ML" href="https://app.qwak.ai/" target="_blank">JFrog ML</Anchor> → **Feature Sets**.
2. Select a Feature Set.
3. Go to `Alerts` tab.
4. Click `Create new alert`.
5. Select `Failure` to be notified of failed feature set ingestions.
6. Choose the relevant alert channels to receive notifications. (If no channels appear, go to [Alert Integrations](/docs/jfrog-ml-alerts "JFrog ML Alerts") to add them.)

   ![feature-set-alert-dashboard.png](../image/uuid-755425ea-ff3e-5cdb-b588-b925ddb2e129.png)
7. Optionally, add a description for the alert, which will be included in the notification body.

   ![description.png](../image/uuid-7ae4ba74-4bd6-10fd-c855-86cc0bbc1e75.png)
8. Save the Alert.
<Callout icon="❗️" theme="error">
**Important**

General constraints

* Alerts must be connected to at least one channel.
* Alert channels may not be deleted if an alert is connected to them.
</Callout>


### Tags and Priority

![tags-and-priority.png](../image/uuid-55930c08-5034-1652-bb02-306bc7ff6ea3.png)

#### Priority

You can assign an alert priority to your channel. Each priority level will be mapped appropriately to the corresponding integration.



<Table>
  <thead>
    <tr>
      <th>
        JFrog ML
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



#### Tags

You can define up to 20 text tags for your Opsgenie alerts, with each tag having a maximum length of 50 characters.

<Callout icon="❗️" theme="error">
**Important**

Can be used **only** for the Opsgenie integration.
</Callout>
