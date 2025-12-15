---
title: 'Shadow AI Detection: Governing Unmanaged Models ​'
deprecated: false
hidden: false
metadata:
  robots: index
---
## Introduction

Uncontrolled use of AI models introduces security, compliance, and cost risks.
​
JFrog’s Shadow AI detection helps identify and bring unmanaged AI models into the AI Catalog, enabling you to apply governance and security policies.
​
By scanning all artifacts across the JFrog Platform, we identify what models are being used, keeping you up to date with what is in your system.

<Image align="center" border={false} src="https://files.readme.io/8c4b168abc8ffa36c8195a41ffde5a70502868211958ba783d647a2aca768938-shadow_ai_introduction.png" />

<Callout icon="📘" theme="info">
  Shadow AI Detection is available to all organizations with an active AI Catalog subscription.
</Callout>

## How does the Shadow AI Detection Process Work?

The ​**Detection**​​ tab provides a single view of all AI models discovered in your JFrog Platform, whether managed or unmanaged. To detect models, your JFrog system uses Xray to scan artifacts.
​
According to your Xray scan settings, repositories are scanned to detect models.
​
If no models appear in the Detection tab, verify that the correct repositories are being scanned by Xray.

<Callout icon="📘" theme="info">
  ​​**For Administrators only: To verify or select repositories to scan:​**
  ​
  ​In the ​Administration​ Module, navigate to ​**Xray Settings**​ > **​Indexed Resources**​​.
  ​
  Browse the list of repositories displayed. If the repositories you want to be scanned are not selected, click ​**Add a Repository**​​.
  ​
  ​
  ​
  Select the repositories you want Xray to scan to detect models and the arrow button to move it into the ​Selected Repositories​​column.
  ​
  ​
  ​
  Click ​Save​​.
  ​
  ​
  ​​​addrepositorytoxrayscan.gif​​
</Callout>
