---
title: Detect Shadow AI
excerpt: Identify and control unmanaged models across your platform
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

<Image align="center" border={true} src="https://files.readme.io/4818ab7b5a1b1af94b8a4ff3a323a72e6997a80dd7bd882859e6fc04bae9f4fa-detection.png" className="border" />

Shadow AI detection gives you the ability to manage and prevent the entry of unvetted AI assets into your system.

<Callout icon="📘" theme="info">
  Shadow AI Detection is available to all organizations with an active AI Catalog subscription.
</Callout>

## How Does the Shadow AI Detection Process Work?

The ​**Shadow AI**​​ page provides a single view of all AI models discovered in your JFrog Platform, whether managed or unmanaged. To detect models, your JFrog system uses Xray to scan artifacts.

​
According to your Xray scan settings, repositories are scanned to detect models.

​
If no models appear in the Detection page, verify that the correct repositories are being scanned by Xray.

<Callout icon="📘" theme="info">
  ​​**For Administrators only: To verify or select repositories to scan:​**

  1. ​In the ​Administration​ module, navigate to ​**Xray Settings**​ > **​Indexed Resources**​​.
  2. Browse the list of repositories displayed. If the repositories you want to be scanned are not selected, click ​**Add a Repository**​​.
  3. Select the repositories you want Xray to scan to detect models and the arrow button to move them into the ​**Selected Repositories**​​ column.
  4. Click **Save**.

  <Image align="center" border={true} src="https://files.readme.io/7fddbb9f1b8afe6e90a73feaa912a8205b50e70b7b7889bd3f22abef61a39199-addrepositorytoxrayscan.gif" className="border" />
</Callout>

Currently, Shadow AI detection supports only model packages.

​In the ​**Detection**​​ page, models are automatically grouped into three categories to help you prioritize review and governance actions:

| Category          | Description                                                                                                                                |
| :---------------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| Managed           | Every instance of the model is either allowed in the AI Catalog or blocked by security policies. No instances of the model are unmanaged.  |
| Unmanaged         | No instances of the model are allowed in ai catalog or blocked by security policies.                                                       |
| Partially Managed | Models approved for use in at least one project and/or blocked in at least one project, but not yet fully governed across other instances. |

For all categories, the system indicates if the model is 'malicious', meaning Xray flagged it as posing a high security risk.

A table of detected models is displayed showing:

* Model ​**name**​​, ​**provider**​​, and ​**type**​​
* ​​​**Governance status**​​ (Managed, Unmanaged, Partially Managed)
* ​**No. of artifacts**​​ in which the model was detected

You can click on a model to open the model pane on the right, where you can see in which artifacts and repositories the model can be found and status of the model for all projects.

## How to Allow Unmanaged or Partially Managed Models

Allowing a model will bring an unmanaged or partially managed model under governance in the AI catalog.

▶ **To allow a detected model:**

1. Either:
   1. In the ​**Detections**​ page, click the ​**Manage**​ button adjacent to the unmanaged model you want to govern (in the ​**Actions**​​ column).  
      or
   2. Click anywhere on the model row, and then in the model pane, click ​**Manage**​​.
2. Select the projects for which you wish to allow the model, and click ​**Save**​​.

   <Image align="left" border={true} width="70% " src="https://files.readme.io/44dc81fe27f3d6c2f9aa996ee40441d66a157abdf73f76cb7a35824922cbb7f5-managemodel_allow.png" className="border" />
3. Click ​**Close**​​ to close the window.

The model is added to the AI Catalog and becomes Managed for that project and appears in the ​**Registry**​​ page. 

<br />

Once all instances of a model are allowed, its overall status updates to Managed. If a model still has unmanaged instances, then its status is indicated as Partially Managed.

<br />

See also: [Discover and Allow Models](doc:discover-and-allow-models)

## Keep Your System Secure

Shadow AI enables you to detect all AI assets in your system. You may need to block specific AI assets due to security and compliance reasons. To do this you need to block the assets via security policies.

### How Does Shadow AI Integrate with JFrog Security?

Shadow AI Detection integrates tightly with JFrog’s security components:

* Detection:
  * **​​JFrog Xray:** ​​Provides detection data, model-to-artifact mapping, and malicious flags.
  * ​**JFrog Advanced Security (JAS):** Identifies external API calls through source-code analysis.
* Block Policies:
  * For **​local repositories​​:** Blocking applies Xray's Download Block policy.
  * For​​ **remote repositories​​:** Create a Curation by label policy that blocks the model from the cache in remote repositories.

​Both these policies block future downloads of the blocked model for that repository.

<Callout icon="📘" theme="info">
  In the AI Catalog, models detected by Xray display an indicator showing “Detected in x artifacts” along with the associated risk level.
</Callout>

## How to Block Models

▶ **To block a model:**

1. In the AI Catalog, open the ​**Detection**​​ page.
2. Either:
   1. Select the row of the model to be blocked; the Model details pane is displayed.
   2. Click **Manage**; the Manage Model pane is displayed.

      or
   * Click the **Block** button to the left of the model's details; the Manage Model pane is displayed.
3. In the **Block this Model** section, select the projects and or the repositories not assigned to project for which to block the model.

   <Image align="left" border={true} width="70% " src="https://files.readme.io/661918f5ae90613b774103274c013ad900520ed8a0cc4c5ba445cabf6d04bc8f-managemodel_block.png" className="border" />
4. Click **Save**.​​
5. Click **Close**.
