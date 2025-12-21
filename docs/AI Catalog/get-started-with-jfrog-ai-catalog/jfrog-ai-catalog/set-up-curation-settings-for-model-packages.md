---
title: Set Up Curation Settings for Model Packages
deprecated: false
hidden: false
metadata:
  robots: index
---
This guide describes how to set up the curation and governance settings for open source models within the JFrog AI Catalog. These settings allow Platform Admins to control which models are allowed for use within the organization, preventing unvetted or risky models from entering your software supply chain.

## Prerequisites

* **User Role:** Only a **Platform Admin** can perform the initial setup and configuration.
* **Subscription:** Ensure your JFrog Platform instance includes entitlement for **JFrog Curation** and **JFrog ML**.

---

## 1. Accessing the Setup Area

To begin configuring curation settings for models:

1.  Log in to the JFrog Platform as an Admin.
2.  In the left sidebar, click on the **AI/ML** link.
    * *Note: If this is your first time accessing this section, it may open the JFrog ML setup form.*

## 2. Initial JFrog ML Setup

If you have not yet set up JFrog ML, you will need to complete the initial configuration form:

1.  **Select a Project:** Choose a JFrog project where JFrog ML will store generated artifacts (such as Docker images and model files).
2.  **Cloud Provider:** Select your preferred cloud provider (e.g., AWS, GCP).
3.  **Region:** Choose the cloud region for deployment.
4.  Click **Get Started** to enable JFrog ML in your account.

---

## 3. Configuring Curation (Allowing Models)

Once JFrog ML is enabled, you can manage which models are available to your developers through the **AI Catalog**.

### The AI Catalog Tabs

Navigate to **AI/ML > Models**. You will see three main tabs:

* **Registry:** Lists models that are **approved** for use in your organization. (Empty by default).
* **Discovery:** A catalog of all available models from supported providers (e.g., Hugging Face, OpenAI) that you can review.
* **Detection:** Displays models found in your artifacts by JFrog Xray scans (Shadow AI detection).

### Discover and Allow Models

To cureate models and add them to your allowed list:

1.  Switch to the **Discovery** tab.
2.  Use the **Filter & Search** functionality to find specific models (e.g., by provider, task, or license).
3.  Review the model details, including vulnerability scans and license information provided by Xray.
4.  **Allow a Model:**
    * Select the model you wish to approve.
    * Click the **Allow** (or Approve) button.
    * You can approve models on a **per-project basis** or globally, depending on your policy configuration.

Once a model is allowed, it moves to the **Registry** tab and becomes available for developers to use securely.

---

## 4. Blocking Risky Models

The primary goal of Curation is to block unauthorized usage.

* **Allow-list Policy:** By default, if no models are in the Registry, developers cannot pull or deploy them via JFrog ML.
* **Automated Policies:** You can define policies to automatically block models based on:
    * **Security Vulnerabilities:** Block models with High or Critical CVEs.
    * **License Compliance:** Block models with non-compliant open source licenses.
    * **Malicious Packages:** Automatically blocked by JFrog Curation's threat intelligence.

## 5. Next Steps

* **Connect AI Providers:** Set up connections to external API providers like OpenAI or Anthropic.
* **Install FrogML CLI:** Developers can install the CLI to interact with the allowed models.
    ```bash
    pip install frogml-cli
    ```
* **Monitor Usage:** Use the Model Dashboards to track how approved models are being used across your organization.