---
title: JFrog ML Alerts
deprecated: false
hidden: false
metadata:
  title: JFrog ML Alerts
  description: >-
    Frog ML allows seamless integration with your preferred monitoring tools for
    instant notifications on ongoing issues both for models and feature sets.
  legacyUUIDs:
    - UUID-dcabea39-0c71-bfcb-f91e-6c8d677eb6d8
    - UUID-eeda75f4-0f5d-0ce6-d509-b7c3c9c52601
  robots: index
---
Frog ML allows seamless integration with your preferred monitoring tools for instant notifications on ongoing issues both for models and feature sets.

Get started:

1. In the **Administration** module, navigate to **AI/ML Settings** > **Integrations**.
2. Connect an alerting provider.
3. Set up alerts with your chosen channel.

## Opsgenie

1. Go to the Opsgenie integration page.
2. Enter your connection information: 
   * Opsgenie region
   * Opsgenie API key
3. Click `Connect`.
   * Validating your credentials will trigger a test alert and close it right away.
   * JFrog ML creates automatically a new channels to send alerts in.

Once the validation succeeds, your Opsgenie Integration will be marked as connected.

### PagerDuty

* Go to <Anchor label="PagerDuty Integration" target="_blank" href="https://app.qwak.ai/qwak-admin/integrations/pagerduty">PagerDuty Integration</Anchor> page
* JFrog ML supports 2 types of PagerDuty integrations:

  * Events V1 integration using your `Service Key`.
  * Events V2 integration using your `Routing Key`.
* Validating your credentials will trigger a test alert and close it right away.
* JFrog ML creates automatically a new channels to send alerts in.
* Once the validation succeeds, your Pagerduty Integration will be marked as connected.

### Configuring Channels

Alert channels are specific channels within each integration to which alert messages will be sent, for example, a specific Slack channel.

Channels are created **PagerDuty** and **Opsgenie** automatically.
