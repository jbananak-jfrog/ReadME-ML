---
title: JFrog ML Alerts
deprecated: false
hidden: false
metadata:
  title: JFrog ML Alerts
  description: Frog ML allows seamless integration with your preferred monitoring tools for instant notifications on ongoing issues both for models and feature sets.
  robots: index
  legacyUUIDs:
    - UUID-dcabea39-0c71-bfcb-f91e-6c8d677eb6d8
    - UUID-eeda75f4-0f5d-0ce6-d509-b7c3c9c52601
---
Frog ML allows seamless integration with your preferred monitoring tools for instant notifications on ongoing issues both for models and feature sets.

Get started by simply:

1. Navigating to <Anchor label="App Integrations" href="https://app.qwak.ai/qwak-admin/integrations" target="_blank">App Integrations</Anchor> under `Settings` → `Integrations`
2. Connecting an alerting provider
3. Setting up alerts with your chosen channel

### Opsgenie

* Go to <Anchor label="Opsgenie Integration" href="https://app.qwak.ai/qwak-admin/integrations/opsgenie" target="_blank">Opsgenie Integration</Anchor> page
* Enter your connection information

  * Your Opsgenie region
  * Your Opsgenie API key
* Click `Connect`

  * Validating your credentials will trigger a test alert and close it right away.
  * JFrog ML creates automatically a new channels to send alerts in.
* Once the validation succeeds, your Opsgenie Integration will be marked as connected.

### PagerDuty

* Go to <Anchor label="PagerDuty Integration" href="https://app.qwak.ai/qwak-admin/integrations/pagerduty" target="_blank">PagerDuty Integration</Anchor> page
* JFrog ML supports 2 types of PagerDuty integrations:

  * Events V1 integration using your `Service Key`
  * Events V2 integration using your `Routing Key`
* Validating your credentials will trigger a test alert and close it right away.
* JFrog ML creates automatically a new channels to send alerts in.
* Once the validation succeeds, your Pagerduty Integration will be marked as connected.

### Configuring Channels

Alert channels are specific channels within each integration to which alert messages will be sent, for example, a specific Slack channel.

Channels are created **PagerDuty** and **Opsgenie** automatically.
