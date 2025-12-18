---
title: Integrating Experiment Tracking Tools with JFrog ML
deprecated: false
hidden: false
metadata:
  title: Integrating Experiment Tracking Tools with JFrog ML
  description: >-
    Experiment tracking is an essential aspect of the machine learning workflow,
    allowing you to monitor and compare different models and runs.
  legacyUUIDs:
    - UUID-56dbf851-5eef-25c7-93fc-f98cf7361c86
    - UUID-feeae0bd-9829-5f42-d8a5-3c77fc3aa2ef
  robots: index
---
Experiment tracking is an essential aspect of the machine learning workflow, allowing you to monitor and compare different models and runs.

JFrog ML provides seamless integration with leading experiment tracking platforms such as Weights & Biases (wandb) and MLflow. This guide will walk you through the process of setting up these integrations within your JFrog ML environment.

## Weights and Biases Integration

When using Weights & Biases (wandb), you store and manage artifacts, which can include datasets, models, and other files, in a centralized database on the wandb cloud service. Here's how to retrieve and utilize model artifacts, metrics, and parameters logged with wandb.

## Setting Up wandb

Before you begin, ensure you have a Weights & Biases account. Follow these steps to integrate wandb with JFrog ML:

1. **Generate a wandb API Key:**

   * Go to your wandb profile settings and create a new API key.
2. **Save the API Key as a JFrog ML Secret:**

   Using the SDK as below, [or using the UI](/docs/secret-management).

   ```
   frogml secrets set --name 'wandb-api-key' --value "<YOUR_WANDB_API_KEY>"
   ```

#### Using a `wandb` in Your Model

To use wandb in your JFrog ML model, you'll need to:

* Initialize wandb with your project and entity details.
* Log in to wandb using the API key stored in JFrog ML Secrets.
* Retrieve and log model artifacts, metrics, and parameters.

Here's a practical example of how to use wandb in your JFrog ML model :

```python
from frogml import FrogMlModel
from frogml.core.clients.secret_service import  SecretServiceClient
import wandb
import frogml
import os

ENTITY_VAR = 'WNB_ENTITY'
PROJECT_VAR = 'WNB_PROJECT'
RUN_ID_VAR = 'WNB_RUN_ID'


class MyCustomModel(FrogMlModel):

    # Initialize Wandb configs from environment variables sent at build time.
    def __init__(self):
        self.entity = os.getenv(ENTITY_VAR)
        self.project = os.getenv(PROJECT_VAR)
        self.run_id = os.getenv(RUN_ID_VAR)

    def build(self):
        pass

    def initialize_model(self):
        """
        Invoked when a model is loaded at serving time. Called once per model instance initialization. 
        Can be used for loading and storing values that should only be available in a serving setting.
        """
        # Access the wandb secret API key from JFrog ML's SecretsManager
        secret_service = SecretServiceClient()
        wandb_api_key = secret_service.get_secret('wandb-api-key')

        # Log in to wandb
        wandb.login(key=wandb_api_key)

        # Initialize a wandb API object
        api = wandb.Api()

        # Replace 'my_entity', 'my_project', and 'run_id' with your specific details
        run = api.run(f"{self.entity}/{self.project}/{self.run_id}")

        # Replace 'model' with the name of your artifact
        artifact = run.use_artifact('model:latest')
        artifact_dir = artifact.download()

        """
        TODO Model initialization from artifact.
        """

        # Retrieve metrics
        metrics = {key: val for key, val in run.history().items() if key not in ['_step', '_runtime']}

        # Log metrics to JFrog ML Build
        frogml.log_metrics(metrics)

        # Retrieve parameters/configurations
        params = run.config

        # Log parameters to JFrog ML Build
        frogml.log_param(params)

    def predict(self, df):
        """
        Invoked on every API inference request.
        """
        pass
```

When initiating this build using the JFrog ML SDK, please include the necessary environment variables by utilizing the `-E` flag, as outlined in the <Anchor label="Build Configurations" title="Build Configurations" href="/docs/build-configurations">Build Configurations</Anchor> documentation page.

## Conclusion

By integrating experiment tracking tools like `wandb` and `MLflow`, you can enhance the capabilities of your JFrog ML-based ML models. This setup enables you to keep track of your experiments, compare results, and ensure that your ML operations are efficient.

<br />
