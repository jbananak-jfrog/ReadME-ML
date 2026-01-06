---
title: Feature Set Alerts
excerpt: >-
  Feature set alerts help you track and maintain the status of batch feature
  sets.
deprecated: false
hidden: false
metadata:
  title: Feature Set Alerts
  description: >-
    Feature set alerts help you track and maintain the status of batch feature
    sets.
  legacyUUIDs:
    - UUID-c66db99d-a45e-a926-b2f4-ef3b60704065
    - UUID-4c45e900-4f7f-b00a-b5a2-eb89b9b71c83
  robots: index
---
Currently, only alerts for batch feature set failures are supported. These alerts notify you when a batch feature set ingestion fails, enabling you to take immediate action.

<Callout icon="📘" theme="info">
  _**Monitoring Integrations**_

  Before setting up alerts, please make sure that the relevant integrations and channels are configured. For more information, please see the [Alert Integrations](/docs/jfrog-ml-alerts) guide.
</Callout>

### Creating Feature Set Alerts

1. Navigate to <Anchor label="JFrog ML" target="_blank" href="https://app.qwak.ai/">JFrog ML</Anchor> → **Feature Sets**.
2. Select a Feature Set.
3. Go to `Alerts` tab.
4. Click `Create new alert`.
5. Select `Failure` to be notified of failed feature set ingestions.
6. Choose the relevant alert channels to receive notifications. (If no channels appear, go to <Anchor label="Alert Integrations" title="JFrog ML Alerts" href="/docs/jfrog-ml-alerts">Alert Integrations</Anchor> to add them.)

   <Image alt="feature-set-alert-dashboard.png" border={false} src="https://files.readme.io/6c28c289c4568a78fc0e54c8d50ed4ef64048d88080f6b98f5142f91e9e4322e-uuid-755425ea-ff3e-5cdb-b588-b925ddb2e129.png" />
7. Optionally, add a description for the alert, which will be included in the notification body.

   <Image alt="description.png" border={false} src="https://files.readme.io/5bc052217636a4025499a3f9c9c7c4255aa67f52ea0c3e0992c92fd2e4692808-uuid-7ae4ba74-4bd6-10fd-c855-86cc0bbc1e75.png" />
8. Save the Alert.

<Callout icon="❗️" theme="error">
  **Important**

  General constraints

  * Alerts must be connected to at least one channel.
  * Alert channels may not be deleted if an alert is connected to them.
</Callout>

### Tags and Priority

<Image alt="tags-and-priority.png" border={false} src="https://files.readme.io/e589ebcff4550ef707e21e1917c920d256f3600f5f56ddd3b1953575c333e0e9-uuid-55930c08-5034-1652-bb02-306bc7ff6ea3.png" />

#### Priority

You can assign an alert priority to your channel. Each priority level will be mapped appropriately to the corresponding integration.

| JFrog ML | Opsgenie | Pagerduty | Slack    |
| :------- | :------- | :-------- | :------- |
| Critical | P1       | critical  | Critical |
| High     | P2       | error     | High     |
| Moderate | P3       | warning   | Moderate |
| Low      | P4       | info      | Low      |
| Info     | P5       | info      | Info     |

#### Tags

You can define up to 20 text tags for your Opsgenie alerts, with each tag having a maximum length of 50 characters.

<Callout icon="❗️" theme="error">
  **Important**

  Can be used **only** for the Opsgenie integration.
</Callout>
