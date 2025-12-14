---
title: Connect AI Providers
excerpt: >-
  A Connection stores the credentials required to access an external API model
  provider, and can be established with providers such as OpenAI, or other cloud
  services such as Amazon Bedrock.
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  title: Connect AI Providers
  description: >-
    A Connection stores the credentials required to access an external API model
    provider, and can be established with providers such as OpenAI, or other
    cloud services such as Amazon Bedrock.
  legacyUUIDs:
    - UUID-2c846677-c49c-db81-d8a8-ff9f3ffe1881
    - UUID-dc048153-9d27-b3ab-e41a-ba9153e1409b
    - UUID-8c6f5b8d-9ce0-e9c1-e377-ce6c03f19f04
    - UUID-79ba98b5-f592-daed-3f37-209d0bba01b9
    - UUID-c068368d-5a22-9d19-f833-33817853ea07
    - UUID-e9c72368-1e48-1697-6793-f620e2f15c2f
    - UUID-1289f52f-4576-bb98-7cdd-c3f7b3064574
    - UUID-97727780-6781-59d9-ca78-db1df075101f
  robots: index
---
A **Connection** stores the credentials required to access an external API model provider, and can be established with providers such as OpenAI, or other cloud services such as Amazon Bedrock.

In the _Connections_ page, you can view a list of all the connections defined in your system.

<Callout icon="📘" theme="info">
  **Note**

  The _Connections_ page is only available to platform admins.
</Callout>

<Image alt="connections.png" border={false} src="https://files.readme.io/9d8003abfcd79d3733037c33ef2209e94cc48cd6d65e37821629eea6f967df96-uuid-7e511d8a-5063-80df-e79f-286f041f8e9c.png" />

To view the _Connections_ page:

1. In the JFrog platform, select the **Administration** module.
2. Scroll down to the bottom of the left menu bar and select **AI/ML Settings** > **Connections**.

In the _Connections_ page, you can:

* Sort ascending/descending by Secret name, Environment, or time of creation
* Filter by Connection name, Provider, Project or Environment
* Create a new secret
* Delete a secret
* Edit a connection (change the associated secret)
* Pin any of the columns to the left/right
* Group by project
* Show/hide columns

{/* See also:

[Create a New Model Provider Connection](/docs/create-a-new-model-provider-connection "Create a New Model Provider Connection")

[Create a New Secret](/docs/create-a-new-secret "Create a New Secret")
*/}

## Create a New Model Provider Connection

Each model provider-project pair requires a unique connection.

When you select a project for allowing a model, and you do not have any models from the same provider already allowed for that project, you must create a new connection.

**To create a new connection:**

EITHER:

1. In the _Allow Model Usage_ pane, enter a unique **Connection name**.

   <Image align="center" alt="createconnection_allowmodelusage.png" border={false} src="https://files.readme.io/2b35d96ffb6292a18e3456e2995e3f48ca17b77f49799dd17c4de3ceb2e7b6ff-uuid-29575b5f-c35e-a875-5cd9-26bbc11b60f0.png" />
2. Select a secret from the **API Key as Secret Name** dropdown list or <Anchor label="Create a New Secret" title="Create a New Secret" href="/docs/create-a-new-secret">Create a New Secret</Anchor>.
3. Click **Create connection**. The "_Connection created successfully_" notification is displayed.

OR:

1. In the Administration module menu bar, select **AI/ML Settings** > **Connections**.

   <Image alt="createnewconnection_admin.png" border={false} src="https://files.readme.io/a4a376d61d8b9477032da9ed4a6233a0baaab35bce8d6b60030d4376427882b8-uuid-13eca0a8-3e13-1944-5679-e21c4ea4f2d3.png" />
2. Click **Create new connection**.
3. Enter a unique **Connection name**, the project and the model provider for the connection.
4. Select a secret for the connection from the **API Key as Secret Name** dropdown list or <Anchor label="Create a New Secret" title="Create a New Secret" href="/docs/create-a-new-secret">Create a New Secret</Anchor>.
5. Click **Save**.

## How to Get API Keys for External Providers

API keys serve as unique identifiers used to authenticate requests from external applications or users to the service provider's API. To enhance your JFrog ML workflows with integrated trading and AI models, acquiring API keys from external providers, such as Gemini and OpenAI, is essential. These keys enable secure and efficient access to powerful functionalities, ensuring smooth operations and automation.

You can integrate several accounts from OpenAI, AWS, and Anthropic, each with your own unique API keys or authentication tokens. Each of these provider accounts can manage multiple models offered by the respective providers.

## Create a New Secret

This procedure outlines the steps to create a new secret, which is used to securely store sensitive credentials such as API keys and access tokens. These secrets enable the JFrog platform to authenticate and establish secure connections with external model providers.

**To create a new secret:**

1. In the _Allow Model Usage_ pane, click **Create new secret**.

   <Image alt="createnewsecret.png" border={false} src="https://files.readme.io/740d60fbb5bba070e491ad73871321e3b8e70c3694f57bddd312e1a5a0a70aa2-uuid-52761a2e-a9cb-e72d-0913-9b903ba655d8.png" />

2. Enter a **Secret name** and the **Secret value**.

   <Callout icon="📘" theme="info">
     **Note**

     For important information about choosing a name for your secret, see <Anchor label="Secret Management" title="Secret Management" href="/docs/secret-management">Secret Management</Anchor>.
   </Callout>

3. Click **Save secret**. The secret is now displayed in the **API Key as Secret name** box.
