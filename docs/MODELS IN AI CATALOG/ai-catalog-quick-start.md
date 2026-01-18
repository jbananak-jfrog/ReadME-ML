---
title: 'Quick Start  '
excerpt: Follow these steps to start managing your AI assets.
deprecated: false
hidden: false
metadata:
  title: Getting Started
  description: >-
    Once you have set up your JFrog ML account and your AI catalog entitlement,
    all you need to do to start allowing models is follow these simple steps:
  legacyUUIDs:
    - UUID-b67e32b5-672a-1774-5aae-edf70ab6a2a4
    - UUID-622ac15d-69f5-5745-e323-4a476ff55b0e
    - >-
      UUID-622ac15d-69f5-5745-e323-4a476ff55b0e_UUID-ea3a6fcc-67be-f602-cb97-abb587ec9cd0
    - UUID-220fcfe1-76a3-d592-77fe-ab422c8067b1
    - UUID-4771c876-6465-e8a5-841a-169aeb05ceed
    - >-
      UUID-4771c876-6465-e8a5-841a-169aeb05ceed_UUID-a5e35879-2d88-5f0f-b9cc-ad2df4eacaf5
  robots: index
---
Once you have set up your JFrog ML account and your AI catalog entitlement, all you need to do to start allowing models is follow these simple steps:

## Workflow for Using the AI Catalog

<Image alt="AIWORKFLOW1HORIZONTAL2.png" border={false} src="https://files.readme.io/83b9af02da8e5642f158bf939206e339fe2ab1b2b6489cdae811633b61074907-uuid-828e705f-fb86-8866-f036-653a28178d36.png" />

### Access the AI Catalog

From the JFrog Platform menu, select **AI/ML**. The **Registry** page opens by default.

The AI Catalog has three main windows:

* **Registry:** The default view, this lists the models approved for use in your organization. If you are a new user, this tab is empty until models are approved.
* **Discovery:** Explore all available models provided by approved (supported) providers that you can review and approve for use.
* **Detection:** View and manage all package* type models found in your organization’s artifacts as scanned by JFrog Xray.

To explore all available models, switch to the **Discovery** page. This page displays the full list of models that can be reviewed and allowed for use within your organization.

<Image alt="discovery_callouts_new.png" border={false} src="https://files.readme.io/b5d1f24f9cfdd91ccd1975576539062b1a510ba5f3e4f550188d27a6da8e81f1-uuid-b8bdfc28-e136-b233-92ad-7dc8947fb882.png" />

<Callout icon="📘" theme="info">
  **Note**

  * While all users can consume allowed models, only Admin level users have the permission to decide which models are allowed for use within your organization.
  * By default, on first use, no models have been allowed.
</Callout>

### Finding Your First Model

Since the **Registry** tab is empty on first use, your first action is to switch to the **Discovery** page. Browse the comprehensive catalog of external APIs, model packages, and your own custom models to discover the best fits for your project requirements.

Use the [**Filter & Search**](/docs/discover-and-allow-models#searching-and-filtering-ai-models) functionality to quickly pinpoint the most suitable models.

The JFrog platform automatically scans each model for vulnerabilities, and offers transparent license information to help you avoid compliance issues.

<Image alt="modeldetails.png" border={false} src="https://files.readme.io/c338827ad0a4936b782d2feda9208535d8cb7c173f768d837198d11500e1040f-uuid-e38f6c34-b8b8-e8ea-f30b-5ce210887d4f.png" />

<Callout icon="📘" theme="info">
  **Note**

  Even if your AI Catalog is empty, that does not mean your organization is not using any models. It maybe be using unmanaged models. Using the [Shadow AI detection ](/docs/shadow-ai-detection-governing-unmanaged-models)feature, which uses Xray to scan your JFrog artifacts, you can detect which models are already being used, and manage them in the AI Catalog.
</Callout>

### Allow Models

Next, governance becomes straightforward and effective. Admin users can easily allow models (from the <Anchor label="Discovery" title="Discover and Allow Models" href="/docs/discover-and-allow-models">Discovery</Anchor> tab) for secure use within your organization. Models can be approved on a per-project basis, with a comprehensive list of allowed models available on the _[Registry](/docs/discover-and-allow-models)_ _(allowed models)_ page.

<Image alt="registry.png" border={false} src="https://files.readme.io/5f29ba39c68182a139f14e7386eff2f7d44bd9b1d9da029b51129c754526036f-uuid-9a9ffa87-6cb7-5559-4d09-332db660a3dc.png" />

You can create an allow-list of models, ensuring that if a model is not on the list, it cannot be used, thereby preventing unvetted models from entering your supply chain.

### Using an Allowed Model

Once a model has been successfully allowed in a project, it moves into the **Registry** page and is ready for use. The steps for model consumption vary depending on the model type (Package or External API).

<Image alt="usinganallowedmodel_new.png" border={false} src="https://files.readme.io/843add4b7c3092c4accbe46accb7d3ee4172ab7c75d2bb1bf6e0392c63b2a718-uuid-97c3e7f7-146f-bd6f-6b8f-bf743f340714.png" />

See: [Allow Your First Model](/docs/discover-and-allow-models#allow-your-first-model) | <Anchor label="Discover and Allow Models" title="Discover and Allow Models" href="/docs/discover-and-allow-models">Discover and Allow Models</Anchor> | <Anchor label="Get Started with JFrog ML" title="Get Started with JFrog ML" href="/docs/get-started-with-jfrog-ml">Get Started with JFrog ML</Anchor>

## Prerequisite: Set Up Curation Settings for Model Packages

This procedure outlines the **prerequisite steps** to enable the use of **model packages** on your platform. Specifically, it involves configuring the curation settings in the administration module.

**To set up curation settings for model packages:**

1. Navigate to the curation settings: Select **Administration** > **Curation Settings** > **General**.
2. Toggle the **Curation On** switch to **ON**.

   <Image alt="generalcurationsettings.png" border={false} src="https://files.readme.io/f510882210e66935fde8abe1a3fb135a1b71c2756fa67a6a698123c7e1cc9488-uuid-0ba2785a-f59a-4165-4377-d92a96a8bc51.png" />
3. Click **Enable repositories** to navigate to the _Remote Repositories_ page.
4. Verify the **PackageType**. Ensure that HuggingFaceML is toggled **ON**.

   <Image alt="enable_package_type.png" border={false} src="https://files.readme.io/4166d946f0869f019052e907fbcdab1220ccdb9a50a56df5ebfb83149c34a134-uuid-c4e98f40-913f-b505-7bb7-427a0bec44bf.png" />
5. Click the package type row to view the package type's repositories.
6. Make sure all the repositories in the package type are also enabled. If any are not enabled, a notification is shown at the top, for example, "_Connect package type status: Partially Connected_".

The prerequisite Curation Settings setup is now complete. If required, you can now return to your model page in the AI Catalog, and continue to enable use of your open source model.

<Callout icon="❗️" theme="error">
  **Important**

  Failure to activate these curation settings may result in the following error when attempting to add a model package. To resolve this error, ensure both settings are activated as described.

  <Image alt="curationerror.png" border={false} src="https://files.readme.io/929da6052201da6b2a2360dc7233c3ca3b0f803e03de5732edb9a209664a5b69-uuid-96821bba-e102-d87a-fa81-0d0dc9df5e09.png" />
</Callout>

<br />
