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

   <Image align="center" alt="generalcurationsettings.png" border={true} src= "https://files.readme.io/094ecb76990c1d412dd38bb696c97ed17638f6ec952a06b8a23dceee0ebeaf07-.generalcurationsettings.png" />

1. Click **Enable repositories** to navigate to the _Remote Repositories_ page.

2. <br />

3. Verify the **PackageType**. Ensure that HuggingFaceML is toggled **ON**

   <Image border={false} src="UUID-c4e98f40-913f-b505-7bb7-427a0bec44bf" />

4. Click the package type row to view the package type's repositories.

5. Make sure all the repositories in the package type are also enabled. If any are not enabled, a notification is shown at the top, for example, "_Connect package type status: Partially Connected_".

The prerequisite Curation Settings setup is now complete. If required, you can now return to your model page in the AI Catalog, and continue to enable use of your open source model.

> **Important**
>
> Failure to activate these curation settings may result in the following error when attempting to add a model package. To resolve this error, ensure both settings are activated as described.
>
> <Image border={false} src="UUID-96821bba-e102-d87a-fa81-0d0dc9df5e09" />
