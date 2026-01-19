---
title: Use Models in Your Code
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
  **Note**

  Note that the integration process varies depending on the model type. Follow the procedure according to the model that you are invoking.
</Callout>

## External API Models

**▶ To use an allowed external API model:**

When a model is allowed, the **Use Model** button appears in the _Model information_ window with the relevant code snippets.

1. Navigate to AI/ML > Registry.

2. Click the project for which the model is allowed in the projects list on the left, and then select (double-click) the model from project's allowed models' list.

   For example:

   <Image align="center" border={true} src="https://files.readme.io/39ba04e9dcf697b8841ce1f4f3e066bd0d455f6791222d4f0222dd9131659725-usemodels_modelsinproject.png" className="border" />

   The model dashboard is shown.

   <Image alt="modeldashboard.png" border={false} src="https://files.readme.io/d617d3968c9e7b6abb6eb1e36d0cee8c25fb9cce1bc822f7d7a8cbcada4b9f93-uuid-77f9b35c-94e6-6893-516a-e4b0f63f54cf.png" />

   For details on how to read the dashboard to monitor and interpret the model's performance, refer to [Use the Model Dashboards](/docs/use-the-model-dashboards)

   Note that the dashboard only shows data when there is traffic.

3. Click **Use Model**. The _Use Model_ pane is displayed. Here you are able to generate a token and insert it into the code snippet for the framework of your choice.

4. Browse through the example code snippets for different libraries and frameworks (Python, Javascript, cURL).

   <Image align="center" border={false} src="https://files.readme.io/5bf27b5bb54b29c4bab7af26d3a168d1c56af8237ad526ce9169ee80000afe0b-use_model.png" />

   Note that this code snippet shown includes a placeholder for the `api_key` for the token you are about to generate, and the `model` name, which includes the name of the connection.

   ```python
   from openai import OpenAI

   client = OpenAI(
     api_key="your_jfrog_api_key",
     base_url="https://a0smkltuxjyst.ml.jfrog.io/v1"
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

5. Click **Generate a token**. The _Set Up A Generic Client_ pane is displayed.

<Callout icon="📘" theme="info">
  **Note**

  Keep the default repository.
</Callout>

6. In the **Configure** tab, enter your JFrog account password and click **Generate Token & Create Instructions**. The token is displayed.
7. Click **Copy**.
8. Click **Done**.
9. Select the correct framework (Python/Javascript/cURL), and note that the token has been inserted into the `api_key`.

   Example `Python`

   ```python
   from openai import OpenAI

   client = OpenAI(
     api_key="abcdefghijklmnopqrstuvwxyz1234567890_tokenexample",
     base_url="https://a0smkltuxjyst.ml.jfrog.io/v1""
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
* **Deploy:** Deploying the model through this platform involves configuring it to run within a managed infrastructure. Once deployed, the model can be accessed and used across all your applications, facilitating consistent performance and scalability. For instructions, see [Deploy Model Packages](doc:deploy-model-packages-open-source).

**▶ To use the allowed model only with the transformers library:**

1. On the **Registry** page, either click the allowed model in the list or select the model from under the relevant project on the left.

   <Image align="center" border={false} src="https://files.readme.io/7753c33c2fd3d84f40c768588648f662d85d51f0805f3de42c559b93f3ffc8b6-use_model_package.png" />

   <br />
2. Click **Use Model**. The _Use model_ pane is displayed. Here you define how to use this model securely with your framework of choice.

   <Image align="center" alt="usemodelbutton.png" border={false} src="https://files.readme.io/62cc556cd4472a3400d41c26265f4e1a966cf6cfc8f153df02d66a4885db200a-usemodel_pane_package.png" />

<Callout icon="📘" theme="info">
  **Note**

  Keep the default repository.
</Callout>

4. Click **Configure** to configure your SDK to pull this model. The _Set Up A HuggingFaceML Client_ pane is displayed. These steps tell the system where to take the model from.
5. Enter your JFrog account password and click **Generate Token & Create Instructions**.
6. Follow the instructions on the Configure tab:

   1. **Copy** the code snippet with the `export` commands into your code interface. This configures the Hugging Face client to work with Artifactory, and adds your repository.

      ```python
      export HF_HUB_ETAG_TIMEOUT=86400
      export HF_HUB_DOWNLOAD_TIMEOUT=86400
      export HF_ENDPOINT=https://jmlsoleng.jfrog.io/artifactory/api/huggingfaceml/research-research-huggingface-remote
      ```
   2. **Copy** and run the code snippet with the export token (to authenticate the Hugging Face client with Artifactory).

      ```python
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
