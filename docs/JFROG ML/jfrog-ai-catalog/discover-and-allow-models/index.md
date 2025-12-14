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

When you enter the AI Catalog, you can explore various models available for use in your projects.

## Managing Allowed Models

After you have allowed your first model, you will see that the allowed models all appear in the **Allowed Models** tab.

* For instructions how to allow your first model, see <Anchor label="**Allowing Your First Model**" title="Allow Your First Model" href="/docs/allow-your-first-model">**Allowing Your First Model**</Anchor>.
* To allow additional models, the same instructions can be followed. Note, however, that if the **model provider is already associated with the project** (meaning that another model from the same provider is associated with the project), the **connection is already set up** and does not need to be re-selected.

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

In order to help you find the best model for your project in the AI Catalog, in the _Models_ page you can search for and filter models by:

* **Free text**: Either in the filter panel or the search bar, enter text to search.
* **Allowed status**: Select to display only allowed models.
* **Model type:** Select a specific model type to display. Options: All/Custom/External/Package.
  ![](https://files.readme.io/e639adbcaf0a8238835b684242f3f918afaa515cc2628c3ae5d19ed1be92a197-uuid-71a3b4c8-054f-61a8-0d00-4c55a681a6a3.png)

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

   1. From the JFrog platform menu, select **AI/ML** > **Models**. The Models page opens. If this is your first model, the **Allowed models** tab is empty.
   2. Select the **Discover AI Catalog** tab to view all available models.

      <Image alt="models_page.png" border={false} src="https://files.readme.io/526d6c2fdf3acb6835f76cb07164185627c7525b33c5080bcacbb9696726c708-uuid-b96e380e-cfad-f41a-319f-d151e7836fd0.png" />
   3. Browse through the catalog to find the model you want. You can [search and filter](/docs/discover-and-allow-models) to locate a specific model or model type.
   4. Click the model you wish to allow. The model details are displayed. To enable your developers to use this model, assign it to a project, connect it (for external models), and allow it.
2. **Select the project and configure model allowance:**

   1. In the model's detail page, click as follows:

      * **For External models:** Click **Connect & allow**
      * **For Open-source models:** Click **Allow**

| ![](https://files.readme.io/d080a4d055e4b7984f2571fda016b71cf159742f3a5b7912dc9ee59d3cb5e150-uuid-8831faf2-28ba-a271-cae0-7600ca750219.png) | ![](https://files.readme.io/fbf64abb2989205a4d9245714e8d40c37876ecaaa6f43cb6f3e8edc11b81df9a-uuid-8b6ad253-c7ff-9e1d-601d-9faa5087e539.png) |
| :------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------ |

<Callout icon="📘" theme="info">
  **Note**

  The Allow/Connect & allow buttons appear twice on the page and you can use either.
</Callout>

2. In the _Allow Model Usage_ pane, select the project from the drop-down list.

<Columns layout="auto">
  <Column>
![](https://files.readme.io/d080a4d055e4b7984f2571fda016b71cf159742f3a5b7912dc9ee59d3cb5e150-uuid-8831faf2-28ba-a271-cae0-7600ca750219.png)
  </Column>

  <Column>
    *Lorem ipsum dolor sit amet, consectetur adipiscing elit*
  </Column>
  
  <Column>
    > Ut enim ad minim veniam, quis nostrud ullamco
  </Column>
</Columns>

| Select Project for an External Model                                                                                                        | Select Project for an Open Source Model                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| ![](https://files.readme.io/66ad1650cfba4bc4ae88cdf7ea6e7478eade8744b48b5a9582eace551a8b3021-uuid-fe2ab1f2-b02d-cd83-c4b5-72ff9459cd58.png) | ![](https://files.readme.io/f9fffb90ecedbcfedcf936fbeb77dbaeaaf8e8bc0d658889051cd2e6ac9dec6a-uuid-7a3f611c-65bb-095e-22f3-d950608454b9.png) |

3. Based on your model type, perform the required configuration and complete the approval (allowance):

   * **For Open-source Models only:** Click **Allow**. The Allowed models tab is displayed.
   * **For External API Models only:**  First <Anchor label="create a new connection" title="Create a New Model Provider Connection" href="/docs/create-a-new-model-provider-connection">create a new connection</Anchor>¹² for the project, and then click **Allow**.
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
