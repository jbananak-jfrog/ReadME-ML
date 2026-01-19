---
title: Discover and Allow Models
deprecated: false
hidden: false
metadata:
  title: Discover and Allow Models
  description: Exploring the AI Catalog
  legacyUUIDs:
    - UUID-44ced790-1ad7-019b-800e-1be6012e3b97
    - UUID-9e9c0f7f-9c3d-4c1d-3501-d0487955eefe
    - >-
      UUID-9e9c0f7f-9c3d-4c1d-3501-d0487955eefe_UUID-424d2216-9a4d-9399-fe41-78b11a6715fd
    - UUID-b8c4fd72-aab5-a76b-c972-29565c9cf5c8
    - UUID-d791b6da-360d-8fea-3395-8361ccd20b84
    - >-
      UUID-d791b6da-360d-8fea-3395-8361ccd20b84_UUID-4e2756b2-840e-704f-0e52-47bcfd475d41
    - UUID-0a859bfb-cc61-c071-b684-2b91569b8ce4
    - UUID-b5f29c98-d501-fedc-1c53-d5c02670fd18
  robots: index
---
## Exploring the AI Catalog

The AI Catalog enables you to fully manage all AI assets in your organisation, You can:

* Explore the various models available for use in your organization in the **Discovery** page.
* You can see which models have already been allowed for use in your projects inn the **Registry** page.
* Detect **all** AI assets that are used in your system, but are unmanaged by AI catalog, and possibly even unvetted by your organization, in the **Detection** page. For more information about detecting unmanaged AI assets, see [Detect Shadow AI](/docs/shadow-ai-detection-governing-unmanaged-models)​​​.

## Managing Allowed Models

After you have allowed your first model, you will see that the allowed models all appear in the **Registry** page.

* For instructions how to start allowing models, see <Anchor label="**Allowing Your First Model**" title="Allow Your First Model" href="#allow-your-first-model">**Allowing Your First Model**</Anchor>.
* To allow additional models, the same instructions can be followed.
* <Callout icon="📘" theme="info">
    When allowing models, if the **model provider is already associated with the project** (meaning that another model from the same provider is associated with the project), the **connection is already set up** and does not need to be re-selected.
  </Callout>

## Managing External APIs

For external APIs, the AI Catalog provides a secure gateway that centralizes access:

* Instead of scattering API keys, you manage secure connections governed by JFrog's permission model.
* After model access has been allowed for a specific project, all traffic is tracked and audited.
* This gives you full visibility and control over usage and performance in real time.

## Handling Model Packages

For model packages, the AI Catalog streamlines the path to production:

* You can enforce granular, policy-based controls to create allow-lists.
* This ensures developers only use models that are vetted and compliant.

## Searching and Filtering AI Models

In order to help you find the best model for your project in the AI Catalog, in the **Discovery** page you can search for and filter models by:

* **Free text**: Enter the search term in the search bar. Click enter and either select the required asset or click "Show all results for "xxx". The filter panel on the left opens for further filtering.

  <Image align="center" border={false} src="https://files.readme.io/cbc15d755581d06346e6ef60882af890ce9118b8e0b75593e61a54afc1ade203-discoverysearch.png" />
* **Status**: In the filter pane, Select to display allowed and or live models.

  <Image align="center" border={false} src="https://files.readme.io/125e7691977646aa6af5276fe81f08ca38f551584fe17f934ad70631bb1c9614-discoverysearchandfilter.png" />
* **Model type:** Select a specific model type to display. Options: All/Custom/External/Package.

The filters selected are shown across the top of the models, and the search text remains in the free text box in the Filters panel:

<Image alt="filter2.png" border={false} src="https://files.readme.io/72286d78a1c5df5a9ead1ae122eb56f28d13a2c4fe03f9991d2de4e01ac1e24f-uuid-cf3cf075-2a48-86b7-5e61-e3b788ab7a30.png" />

Click **Clear all** to remove all filters.

## Allow Your First Model

<Callout icon="❗️" theme="error">
  **Important**

  Only Admin users have permission to allow models.
</Callout>

The AI Catalog provides access to thousands of models. This guide outlines the straightforward process for exploring and evaluating a diverse range of AI models, helping you identify and allow the most suitable options for your projects.

1. **Select a model**:

   1. From the JFrog platform menu, select **AI/ML** > **Discovery**. The Models _Discovery_ page opens. (If you are selecting your first model, the _Registry_ page is empty.)

   <Image align="center" border={false} src="https://files.readme.io/ccd43cbb223c801cd2b63912b44a0fcfba8397cca37d009b38e56384e2d040ff-Discovery.png" />

   1. Browse through the catalog to find the model you want. You can [search and filter](/docs/discover-and-allow-models) to locate a specific model or model type.
   2. Click the model you wish to allow. The model details are displayed. To enable your developers to use this model, assign it to a project, connect it (for external models), and allow it.
2. **Select the project and configure model allowance:**

   1. In the model's detail page, click as follows:

      * **For External models:** Click **Connect & allow**

      * **For Model packages:** Click **Allow**

<div style={{ marginLeft: '36px' }}>
  <Columns layout="auto">
    <Column>
      <Image align="center" src="https://files.readme.io/513cdd35e9df27f27749dc64a821f4c63ba471edf10f0b50852376e4ec0c4286-connectandallow_1.png" />
    </Column>

    <Column>
      <Image align="center" src="https://files.readme.io/cddc7e7d7c2e3d3b051adb8bd02872a99157f4bd662a1bc0ecbe0ec0d6a33a8f-opensource_model_information.png" />
    </Column>
  </Columns>
</div>

<ol style={{ listStyleType: 'lower-roman', marginLeft: '20px' }} start="2">
  <li>In the *Allow Model Usage* pane, select the project from the drop-down list.</li>
</ol>

<div style={{ marginLeft: '36px' }}>
  <Columns layout="auto">
    <Column>
      <div style={{ textAlign: 'center' }}>Select Project for an External Model</div>

      <Image align="center" src="https://files.readme.io/e73cbbf0c2846f2fc367e9ac00cdb1177d0aade0042618ff1bf4b0b884697575-allowmodelusage_ext_long_1.png" />
    </Column>

    <Column>
      <div style={{ textAlign: 'center' }}>Select Project for a Model Package</div>

      <Image align="center" src="https://files.readme.io/e08fab9f0ac1560ef5488679ae2323bf6c7c266d0d6b9c74e3845c3bff833f86-opensource_projects_dropdown.png" />
    </Column>
  </Columns>
</div>

3. Based on your model type, perform the required configuration and complete the approval (allowance):

   * **For Model Packages only:** Click **Allow**. The Allowed models tab is displayed.
   * **For External API Models only:**  First [create a new connection](/docs/connect-ai-providers#create-a-new-model-provider-connection)¹² for the project, and then click **Allow**.
4. To Use the Model: See instructions how to <Anchor label="Integrate Models in Your Code" title="Integrate Models in Your Code" href="/docs/integrate-models-in-your-code">Integrate Models in Your Code</Anchor>.

<Callout icon="📘" theme="info">
  **Note**

  You can create a connection without currently allowing the project if you want.
</Callout>

¹ Connections are per provider per project. For more info, see <Anchor label="Connect AI Providers" title="Connect AI Providers" href="/docs/connect-ai-providers">Connect AI Providers</Anchor>.

² When allowing a model for which a connection is already set up between the project and the model provider, you do not need to define a new connection.

## Add Projects to an Allowed Model

This procedure explains how to allow a model for additional projects, after it is already allowed for a specific project.

**To allow a model for an additional project:**

1. In the JFrog platform, navigate to **AI/ML** > **Models** and select the model that you want to allow for an additional project.
2. In the model page, click the **Add** button adjacent to the "Allowed in projects" title and select the project to allow the model for (in the Allow model usage pane).

   <Image alt="allowmodelusage_addbutton.png" border={false} src="https://files.readme.io/452b2c86bdcc118329e086a2ca3059dab86d2e4a54ceed53b738c16b87108944-uuid-414d9bfe-d8b1-6ec6-c392-712ef65d438a.png" />

<Callout icon="✅" theme="okay">
  **Tip**

  A list of the projects that the model is already allowed for is displayed below the Project selection field.
</Callout>

3. Click **Allow**. The project now appears in the "Allowed in projects" list.
