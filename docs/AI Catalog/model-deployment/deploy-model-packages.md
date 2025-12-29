---
title: Deploy Model Packages
deprecated: false
hidden: false
metadata:
  title: Deploy Model Packages (Open source)
  description: >-
    This procedure describes how to easily deploy approved AI models with secure
    connections to external providers, thereby streamlining your AI integration
    process. This procedure is part of the AI Catalog capabilities.
  legacyUUIDs:
    - UUID-59f6ccac-6f54-7e73-1c55-dd51ae2d0bed
    - UUID-37fd0780-6c92-769e-34b3-b648b0a3732c
    - >-
      UUID-37fd0780-6c92-769e-34b3-b648b0a3732c_UUID-c82c1531-9e0e-d8f7-0db5-757f34a075a9
  robots: index
---
This procedure describes how to easily deploy approved AI models with secure connections to external providers, thereby streamlining your AI integration process. This procedure is part of the AI Catalog capabilities.

Deploying a model means actually setting up servers, often GPUs, and getting the models out live in your systems. Before connecting and deploying, you need to select a model and allow its use.

## Key Actions

* **One-Click Deployment:** Quickly deploy allowed models with a single click.
* **Secure Connections:** Establish and manage secure links to external model providers.

## Benefits

* **Simplified Integration:** Accelerate the process of bringing AI applications into production.
* **Ongoing Monitoring:** Keep track of model performance and usage post-deployment.

**▶ To deploy a model package:**

<Callout icon="📘" theme="info">
  **Note**

  If you are deploying a Hugging Face gated model, see the _**[Deploying Gated Models](/docs/deploy-model-packages-open-source#deploying-gated-models)**_ section below.
</Callout>

<Image align="left" alt="deploymodel1.png" border={true} width="80% " src="https://files.readme.io/e8cc1e7b7049184a0c104b6dcc740308cf97d3abddd30c4fee0cc2424aa2510c-uuid-9391de17-3f66-eecd-8b78-77617e80c98b.png" className="border" />

1. Verify that the model name at the top of the _Deploy model_ pane is the model you want to deploy, and also that the project associated with the deployment is the correct project.

2. Select the **Instance type** from the dropdown menu. Refer to [Instance Sizes & ML Credits](/docs/instance-sizes-ml-credits) for detailed information on the available sizes and credits.

3. Select the **Scaling policy** and number of **replicas**:

   * **Autoscaling:** Coming soon (this option will allow the replicas to scale according to demand).
   * **Fixed replicas:** Select this to maintain a fixed number of replicas, according to the number you select. Select either on the **Replicas** bar, or select the **Custom replica count** checkbox and enter a value.

4. Click **Deploy model**. The model Overview page shows the deployment status.

<Callout icon="📘" theme="info">
  **Note**

  While the deployment is in process, a **Cancel deployment** button appears on the right-hand side of the page.
</Callout>

After the deployment has completed successfully you can see the model dashboard and can integrate the model in your code, using the instructions below:

1. In the **Configure** tab, enter your JFrog account password and click **Generate Token & create Instructions**.

2. Click **Deploy**. The deployment status is shown.

3. Once deployment is successful , click **Use Model** again and now the frameworks are enabled (Python, Javascript, cURL).

4. Click **Generate a token**.

5. The _Set Up A Generic Client_ pane is displayed.

<Callout icon="📘" theme="info">
  **Note**

  Keep the default repository.
</Callout>

6. Click **Copy**.
7. Click **Done**.
8. Select the correct framework and note that the token has been inserted into the `api_key`.
9. Copy the code snippet into your code editor.

**Now in the model overview page, you can see the model's usage metrics.**

## Deploying Gated Models

Deploying gated models requires obtaining access approval from Hugging Face before deployment.

**To deploy a gated model:**

1. Enter the Deploy model pane for the required project (as described at the top of this page). Note that it is slightly different.

   <Image align="center" alt="deploygatedmodels.png" border={true} src="https://files.readme.io/6c2a51021badf52e9470c5552f3d5c5ee4ea2ef352e879e88c53c60d848c5b02-uuid-dcaa992a-3997-ca6a-37a5-8dc3940f015f.png" className="border" />
2. Follow the instructions at the top of the pane for getting access approval from Hugging Face.
3. Fill in the other fields as described above, and click **Deploy model**.
