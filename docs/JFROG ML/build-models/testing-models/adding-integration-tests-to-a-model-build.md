---
title: Adding Integration Tests to a Model Build
deprecated: false
hidden: false
metadata:
  robots: index
---
## Validating Serving Artifact

After executing the `build()` function in the model-building process, JFrog ML initiates a critical step - **Validating Serving Artifact**. This involves starting a Docker container encapsulating the newly built model. This container serves two primary purposes:

1. **Initialization**: The `initialize_model()` function is executed to ensure the model serving is correctly set up and started within the container.
2. **Dummy Request Handling**: The container, with its embedded webserver, is tested with a dummy request. This step confirms the container's operational status and its ability to handle incoming requests successfully.

![Models Build Page](UUID-4a40f754-534e-a799-7d35-701739673f86)
*JFrog ML Models Build Page*

## Running Integration Tests

The same container used in the validation phase is also operational in the **Running Tests** phase. This phase is crucial for treating the container as a live model and enables local endpoint predictions. This setup allows for more robust testing, ensuring that the model is fully functional and deploy-ready before it enters shadow or production environments.

To facilitate this, you can execute test predictions using the following syntax:

`integration_tests.py`

```python
from frogml.core.testing.fixtures import real_time_client
from frogml_inference.realtime_client.client import InferenceOutputFormat

def test(real_time_client):
    result = real_time_client.predict(feature_vector, InferenceOutputFormat.PANDAS)