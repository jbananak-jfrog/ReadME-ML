---
title: Set Up Curation Settings for Model Packages
deprecated: false
hidden: false
metadata:
  robots: index
---
This procedure outlines the **prerequisite steps** to enable the use of **model packages** on your platform. Specifically, it involves configuring the curation settings in the administration module.

**▶ To set up curation settings for model packages:**

1. Navigate to the curation settings: Select **Administration** > **Curation Settings** > **General**.

2. Toggle the **Curation On** switch to **ON**.

   <Image align="center" alt="generalcurationsettings.png" border={true} src="https://files.readme.io/67c846925d98157989a4df7e87d8de05af8f60c2450c87f3615c2927b93e1f25-generalcurationsettings.png" className="border" />

3. Click **Enable repositories** to navigate to the _Remote Repositories_ page.

   <Image align="center" border={false} src="https://files.readme.io/7b68b4a590a50c8b4d4fb97f4e553ddd3aa4e85c3c91ee8e477a6f3ed14edce1-enable_package_type.png" />

4. Verify the **PackageType**. Ensure that HuggingFaceML is toggled **ON**

   <Image border={false} src="UUID-c4e98f40-913f-b505-7bb7-427a0bec44bf" />

5. Click the package type row to view the package type's repositories.

6. Make sure all the repositories in the package type are also enabled. If any are not enabled, a notification is shown at the top, for example, "_Connect package type status: Partially Connected_".

The prerequisite Curation Settings setup is now complete. If required, you can now return to your model page in the AI Catalog, and continue to enable use of your open source model.

<Callout icon="❗️">
  **Important**

  Failure to activate these curation settings may result in the following error when attempting to add a model package. To resolve this error, ensure both settings are activated as described.

  <Image align="left" border={true} src="https://files.readme.io/7e5d331ed128bae11dc821c5bc69398339c385759c5b021e7b59b63f63dac3ce-curationerror_copy.png" />
</Callout>

<br />
