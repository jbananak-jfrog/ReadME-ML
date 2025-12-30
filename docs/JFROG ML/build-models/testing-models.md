---
title: Testing Models
deprecated: false
hidden: false
metadata:
  title: Testing Models
  description: 'This section reviews the following topics:'
  legacyUUIDs:
    - UUID-3f6b1f22-6628-4c63-6db7-e64ea78d6c1b
    - UUID-ffe71485-9537-405b-c058-fbda05aa7bc0
    - UUID-9f7b5597-8b7e-4703-a74f-c069c5c31d8d
    - UUID-24f22cba-a52d-263f-fc28-f968daf45e91
    - UUID-b9135a8e-554b-6892-86cd-0df8ff58fe12
    - UUID-3fb9d1f7-af13-c47d-5985-a26471b8b72b
  robots: index
---
This section reviews the following topics:

[Local Testing](/docs/testing-models#local-testing)

[Adding Integration Tests to a Model Build](https://jfrog-enterprise-group.readme.io/ai-ml/docs/testing-models#adding-integration-tests-to-a-model-build)

***

## Local Testing

Local testing before triggering remote builds is essential for optimizing the model development process. This approach enhances efficiency by identifying and resolving errors early in the development cycle, minimizing the time and resources spent on remote builds.

Debugging is more interactive and streamlined locally, allowing quick iteration and error resolution. Additionally, local testing helps validate the entire workflow, ensuring correct configurations and dependencies before incurring potential costs associated with remote builds.

Ultimately, incorporating local testing into the development workflow promotes a more efficient, cost-effective, and error-resistant model building process.

### Local Debugging and Inference

It is possible to easily run and debug your JFrog ML models locally.

The example below contains a simple FLAN-T5 model loaded from HuggingFace. To test inference locally, and can simple run the code below.

<Callout icon="📘" theme="info">
  **Note**

  Please make sure to install the <Anchor label="**frogml-cli**" title="Get Started with JFrog ML" href="/docs/get-started-with-jfrog-ml">**frogml-cli**</Anchor> in your local environment.
</Callout>

### Running Models Locally

We import `from frogml.sdk.model.tools import run_local` and call our local model via `run_local(m, input_vector)`, which invokes all the relevant model methods.

<Callout icon="⚠️" theme="warning">
  **Warning**

  _**Production Models**_

  Please do not leave `from frogml.sdk.model.tools import run_local` imports in production models, as it may affect model behavior. One option would be to create a separate file outside of your `main` directory where you have all the local testing code, including the `run_local` import.
</Callout>

This example will load a model `FLANT5Model` and run inference with a data frame vector of your choice:

```python
import frogml
import pandas as pd
from pandas import DataFrame
from frogml.sdk.model.base import BaseModel as FrogMlModel
from frogml.sdk.model.schema import ModelSchema,ExplicitFeature
from transformers import T5Tokenizer, T5ForConditionalGeneration


class FLANT5Model(FrogMlModel):

    def __init__(self):
        self.model_id = "google/flan-t5-small"
        self.model = None
        self.tokenizer = None

    def build(self):
        pass

    def schema(self):
        model_schema = ModelSchema(
            inputs=[
                ExplicitFeature(name="prompt", type=str),
            ])
        return model_schema

    def initialize_model(self):
        self.tokenizer = T5Tokenizer.from_pretrained(self.model_id)
        self.model = T5ForConditionalGeneration.from_pretrained(self.model_id)

    @frogml.api()
    def predict(self, df):
        input_text = df['prompt'].to_list()
        input_ids = self.tokenizer(input_text, return_tensors="pt")
        outputs = self.model.generate(**input_ids, max_new_tokens=100)
        decoded_outputs = self.tokenizer.batch_decode(outputs, skip_special_tokens=True)
        return pd.DataFrame([{"generated_text": decoded_outputs}])
```

<Callout icon="❗️" theme="error">
  **Important**

  Call `run_local` directly instead of the `model_object.predict()` , as it will not work locally.
</Callout>

To run local inference, add the following code to your model code file:

test_model_locally.py

```python
from frogml.sdk.model.tools import run_local
from main.model import FLANT5Model
from pandas import DataFrame

if __name__ == '__main__':
    # Create a new instance of the model
    m = FLANT5Model()

    # Create an input vector and convert it to JSON
    input_vector = DataFrame(
        [{
            "prompt": "Why does it matter if a Central Bank has a negative rather than 0% interest rate?"
        }]
    ).to_json()

    # Run local inference using the model
    prediction = run_local(m, input_vector)
    print(prediction)
```

<Callout icon="❗️" theme="error">
  **Important**

  For local testing, remember to import `run_local` at the start of your test file, before importing your `FrogMlModel` based class.
</Callout>

### Debugging the Model Life Cycle

Running the `run_local` method calls the following methods in a single command:

* `build()`
* `initialize_model()`
* `predict()`

**Note: The `build` and `initialize_model` functions are called during the first `run_local` run only.**

<Callout icon="❗️" theme="error">
  **Important**

  _**Debugging Input and Output Adapters**_

  Calling the `predict` method locally doesn't trigger the input and output adapters. Please use `run_local` instead.
</Callout>

### Using Proto Adapter

Let's assume that you have the following model class, created an instance and executed the `build` method.

```python
# This example model uses ProtoBuf input and output adapters
class MyFrogMlModel(FrogMlModel):

    def build(self):
                        ...

    @frogml.api(
        input_adapter=ProtoInputAdapter(ModelInput),
              output_adapter=ProtoOutputAdapter()
    )
    def predict(self, input_: ModelInput) -> ModelOutput:
                        return ...
```

In this example, we import a `ProtoAdapter` and use it to perform inference.

```python
from frogml.sdk.model.tools import run_local

# Create a local instance of your model
model = MyForgMlModel()

# ModelInput is the model proto
input_ = ModelInput(f1=0, f2=0).SerializeToString()

# The run_local() method calls build(), initialize_model() and predict()
result = run_local(model, input_)

# ModelOutput is the model proto
output_ = ModelOutput()
output_.ParseFromString(result)  
```

### Running Local Inference

You can test the entire inference code, including input and output adapters, by calling the `execute` function:

```python
# ModelInput is the model proto
input_ = ModelInput(f1=0, f2=0).SerializeToString()

# The execute() calls the predict method with the input and output adapters
result = model.execute(input_)

# ModelOutput is the model proto
output_ = ModelOutput()
output_.ParseFromString(result)
```

***

## Adding Integration Tests to a Model Build

### Validating Serving Artifact

After executing the `build()` function in the model-building process, JFrog ML initiates a critical step - **Validating Serving Artifact**. This involves starting a Docker container encapsulating the newly built model. This container serves two primary purposes:

1. **Initialization**: The `initialize_model()` function is executed to ensure the model serving is correctly set up and started within the container.
2. **Dummy Request Handling**: The container, with its embedded webserver, is tested with a dummy request. This step confirms the container's operational status and its ability to handle incoming requests successfully.

<Image align="center" border={true} src="https://files.readme.io/5de0546d9c7b226e16be042d99dc8d7330a7945bdb57cca71298d10663402444-validating-server-artifact-jfrogml.png" className="border" />

_JFrog ML Models Build Page_

### Running Integration Tests

The **Running Tests** phase keeps the exact container from the Validation Phase operational. This step is critical: by treating the container as a live model, it enables local endpoint predictions to guarantee the model is fully functional and deploy-ready before entering shadow or production.

Execute test predictions using:`integration_tests.py`

```python
from frogml.core.testing.fixtures import real_time_client
from frogml_inference.realtime_client.client import InferenceOutputFormat

def test(real_time_client):
    result = real_time_client.predict(feature_vector, InferenceOutputFormat.PANDAS)
```

<br />

The `real_time_client` is configured with the local endpoint to enable efficient, practical test predictions. This approach enhances your testing practices by ensuring early issue detection and confirming deployment readiness. It is a proactive strategy that aligns with Continuous Integration (CI) workflows to maintain high-quality standards.

<Callout icon="📘" theme="info">
  **Structuring the Tests Directory**

  Please place your integration tests under an `it` directory in the `tests` folder, adhering to the model build directory structure.
</Callout>
