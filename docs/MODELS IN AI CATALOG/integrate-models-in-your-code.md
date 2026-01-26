---
title: Use Models in Your Code
excerpt: ' Integrate allowed models within your code.'
deprecated: false
hidden: false
metadata:
  title: Integrate Models in Your Code
  description: >-
    This procedure explains how to utilize the allowed models within your
    organization's code.
  legacyUUIDs:
    - UUID-ca01bbfb-934d-5807-cf5a-d03d31126850
    - UUID-f2784bab-c363-4065-7f95-dad452b11a4f
  robots: index
---
This procedure explains how to utilize the allowed models within your organization's code.

<Callout icon="📘" theme="info">
  Note that the integration process varies depending on the model type. Follow the procedure according to the model that you are invoking.

  [External API Models](/docs/integrate-models-in-your-code#external-api-models) | [Model Packages](/docs/integrate-models-in-your-code#-model-packages)
</Callout>

## External API Models

**▶ To use an allowed external API model:**

When a model is allowed, the **Use Model** button appears in the _Model information_ window with the relevant code snippets.

1. Navigate to **AI/ML** > **Registry**.

2. Click the project for which the model is allowed in the projects list on the left, and then select (double-click) the model from project's allowed models' list.

   For example:

   <Image align="center" border={true} width="70% " src="https://files.readme.io/5f3dfbc23e0e87b5e135c643cf7884af195f3457deba0af40e0828f9f5e6a647-usemodels_modelsinproject.png" className="border" />

   The model dashboard is shown.

   <Image align="center" alt="modeldashboard.png" border={false} width="70% " src="https://files.readme.io/c9ad77e95f0dcc706d6784399bf29a4245a620fe141ad9b647c0c96aab67184b-modeldashboard.png" />

   For details on how to read the dashboard to monitor and interpret the model's performance, refer to [Runtime Metrics](/docs/ai-catalog-model-dashboards).

   Note that the dashboard only shows data when there is traffic.

3. Click **Use Model**. The _Use Model_ pane is displayed. Here you are able to generate a token and insert it into the code snippet for the framework of your choice.

4. Browse through the example code snippets for different libraries and frameworks (Python, Javascript, cURL).

   <Image align="center" border={false} width="70% " src="https://files.readme.io/d44e4861131702726602f86f9608ca78642c2b4b3edd39aac79295ee451fa792-use_model.png" />

   Note that this code snippet shown includes a placeholder for the `api_key` for the token you are about to generate, and the `model` name, which includes the name of the connection.

   ```python
   from openai import OpenAI

   client = OpenAI(
     api_key="your_jfrog_api_key",
     base_url="https://<your-id>.ml.jfrog.io/v1"
   )

   response = client.chat.completions.create(
     model="OpenAI/gpt-3.5-turbo-1106",
     messages=[
       {"role": "system", "content": "You are a helpful assistant."},
       {
         "role": "user",
         "content": "Explain to me how AI works in one sentence"
       }
     ]
   )

   print(response.choices[0].message)
   ```

   <br />

```python
```

5. Click **Generate a token**. The _Set Up A Generic Client_ pane is displayed.

<Callout icon="📘" theme="info">
  **Note**

  Keep the default repository.
</Callout>

6. In the **Configure** tab, enter your JFrog account password and click **Generate Token & Create Instructions**. The token is displayed.
7. Click **Copy**.
8. Click **Done**.
9. Select the framework (Python/Javascript/cURL), and note that the token has been inserted into the `api_key`.

   Example:

   ```python
   from openai import OpenAI

   client = OpenAI(
     api_key="abcdefghijklmnopqrstuvwxyz1234567890_tokenexample",
     base_url="https://<your-id>.ml.jfrog.io/v1""
   )

   response = client.chat.completions.create(
     model="Gemini/gemini-1.5-flash",
     messages=[
       {"role": "system", "content": "You are a helpful assistant."},
       {
         "role": "user",
         "content": "Explain to me how AI works in one sentence"
       }
     ]
   )

   print(response.choices[0].message)
   ```
10. Copy the code snippet into your code editor. You can now use this model in your applications.

## &#x20;Model Packages

After a model. package has been allowed, the **Use Model** and **Deploy** buttons appear in the _Model information_ window, allowing you to either use the model with the transformers library or to use it in all your applications.

**Using the Model with Your Transformers Library vs. Deploying:**

* **Use Model:** Enables you to integrate the model with the transformers library, you use the model locally within your development environment. This approach is ideal for experimenting, testing, and small-scale applications. For instructions, see below.
* **Deploy:** Deploying the model through this platform involves configuring it to run within a managed infrastructure. Once deployed, the model can be accessed and used across all your applications, facilitating consistent performance and scalability. For instructions, see [Deploy Model Packages](/docs/deploy-model-packages).

**▶ To use the allowed model only with the transformers library:**

1. On the **Registry** page, either click the allowed model in the list or select the model from under the relevant project on the left.

   <Image align="center" border={false} width="70% " src="https://files.readme.io/7753c33c2fd3d84f40c768588648f662d85d51f0805f3de42c559b93f3ffc8b6-use_model_package.png" />

   <br />
2. Click **Use Model**. The _Use model_ pane is displayed. Here you define how to use this model securely with your framework of choice.

   <Image align="center" alt="usemodelbutton.png" border={false} width="70% " src="https://files.readme.io/62cc556cd4472a3400d41c26265f4e1a966cf6cfc8f153df02d66a4885db200a-usemodel_pane_package.png" />

<Callout icon="📘" theme="info">
  **Note**

  Keep the default repository.
</Callout>

4. Click **Configure** to configure your SDK to pull this model. The _Set Up A HuggingFaceML Client_ pane is displayed. These steps tell the system where to take the model from.
5. Enter your JFrog account password and click **Generate Token & Create Instructions**.
6. Follow the instructions on the Configure tab:

   1. **Copy** the code snippet with the `export` commands into your code interface. This configures the Hugging Face client to work with Artifactory, and adds your repository.

      ```shell
      export HF_HUB_ETAG_TIMEOUT=86400
      export HF_HUB_DOWNLOAD_TIMEOUT=86400
      export HF_ENDPOINT=https://jmlsoleng.jfrog.io/artifactory/api/huggingfaceml/research-research-huggingface-remote
      ```
   2. **Copy** and run the code snippet with the export token (to authenticate the Hugging Face client with Artifactory).

      ```shell
      export HF_TOKEN=tokenexample
      ```
   3. Click **Done**.
7. To use the allowed model, please copy the Transformers code snippet into your Python environment:

   ```python
   # Load model directly
   from transformers import AutoTokenizer, AutoModelForCausalLM
   tokenizer = AutoTokenizer.from_pretrained("model_name")
   model = AutoModelForCausalLM.from_pretrained("model_name")
   ```

Now you are ready to use the model directly within your development environment using the Transformers library.

**To use the model with all your applications (Deploy):**

* **See [Deploy Model Packages](/docs/deploy-model-packages) for instructions.** These instructions include how to deploy the model **and** also how to configure the model for use in your developments. After you have deployed and configured the model, you can see the model's usage metrics in the model _Overview_ page.

<br />
