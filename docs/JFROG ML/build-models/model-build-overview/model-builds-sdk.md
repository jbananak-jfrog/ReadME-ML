---
title: Model Builds SDK
deprecated: false
hidden: false
metadata:
  robots: index
---
Data scientists often train models in Workspaces, Jupyter notebooks or locally, and require a seamless process to save, register and manage model versions for production use.

JFrog ML provides the Build Model SDK to address this need, simplifying the transition from model training to deployment, from your local machine or your Jupyter notebook.

### Key Features

#### 1. Build Models from Workspaces

JFrog ML Build SDK simplifies the registration of locally or Jupyter notebook-based model training. It ensures precise versioning, tracking, and effortless transition from research to deployment in production.

#### 2. Python-driven Model Builds

Automate model builds using Python and seamlessly integrate with continuous integration/continuous deployment (CI/CD) pipelines and various automation workflows.

#### 3. Streamlined Versioning for Pre-built Models

Build and register pre-trained models from any Python environment by supplying existing trained model instances, skipping remote build phases.

### Using Build SDK

This document will explore the various options available when using the Build SDK.

In general, there are two choices when working with the Build SDK:

1. Providing a pre-trained model artifact to the `build_model` method.
2. Omitting a pre-built model, in which case the SDK will upload local model files and builds the model on JFrog ML.

```python
from frogml.sdk.frogml_client.client import FrogMLClient
from frogml.sdk.model.tools import run_local

## Creating an instance of the Frogml client
client = FrogMLClient()

## Triggering a build with model files from the local `main` directory
## This option does not provide a pre-built model, and the model is build on JFrog platform
client.build_model(
  model_id='my_example_model',
)

## # Triggering a build with model files from the local `main` directory
## # This option provides a pre-built model, and the build method will not be called remotely.
model = MyFrogmlModel()
model.build()

client.build_model(
  model_id='my_example_model',
  prebuilt_frogml_model=model
)
```

### Folder Structure

<Callout icon="⚠️" theme="warning">
  **Warning** - _**File Structure Requirements**_

  When using the Build SDK, your file and folder structure is preserved when uploading to JFrogML.

  Please ensure to:

  * Avoid Python files with top-level executable statements in the build directory.
  * All code with side effects should be guarded with `if __name__ == "__main__"` blocks.
  * Place shared functionality in properly encapsulated classes and functions.

  Failure to follow these guidelines may cause unintended code execution during the import process.
</Callout>

The Build SDK uploads local model files together with the trained model object. By default, the Build SDK uploads the `main` folder under the current file location.

Make sure that you place your model files in the `main` directory.

```
-> your-model-directory
---- build_sdk_runner.py
---> main
------ model.py
```

It is possible to change the uploaded directory by providing an explicit path as will be described in this document.

### Building Pre-trained Models

Use the Build SDK to build models with an existing instance of a trained model to upload the pre-trained model artifact. This flexibility empowers data scientists to train or fine-tune models within notebooks, effortlessly incorporate the trained versions into the model registry, and deploy them to production environments.

### Creating a Model Instance

In this example, we'll use the Titanic model, which can be found on the <Anchor label="FrogML Examples repository" target="_blank" href="https://github.com/jfrog/JFrogMLExamples">FrogML Examples repository</Anchor>.

Our folder structure will look as follows:

```
titanic
-- run_build.py
-- main
---- __init__.py
---- model.py
---- requirements.txt
```

<Callout icon="📘" theme="info">
  **Note**

  Make sure to import `from frogml.sdk.model.tools import run_local` when using the build SDK. The build command cannot complete without it.
</Callout>

##### requirements.txt

```
pandas
scikit-learn
catboost
```

##### titanic/main/**init**.py

```python
from .model import TitanicSurvivalPrediction

def load_model():
    return TitanicSurvivalPrediction()
```

##### titanic/main/model.py

```python
import os

import frogml
import numpy as np
import pandas as pd
from catboost import CatBoostClassifier, Pool, cv
from catboost.datasets import titanic
from frogml import FrogMlModel
from frogml.sdk.model.schema import ExplicitFeature, InferenceOutput, ModelSchema
from sklearn.model_selection import train_test_split


class TitanicSurvivalPrediction(FrogMlModel):
    def __init__(self):
        loss_function = os.getenv(""loss_fn"", ""Logloss"")
        learning_rate = os.getenv(""learning_rate"", None)
        if learning_rate:
            learning_rate = int(learning_rate)
        iterations = int(os.getenv(""iterations"", 1000))

        custom_loss = ""Accuracy""
        self.model = CatBoostClassifier(
            iterations=iterations,
            custom_loss=[custom_loss],
            loss_function=loss_function,
            learning_rate=learning_rate,
        )

    def build(self):
        titanic_train, _ = titanic()
        titanic_train.fillna(-999, inplace=True)

        x = titanic_train.drop([""Survived"", ""PassengerId""], axis=1)
        y = titanic_train.Survived

        x_train, x_test, y_train, y_test = train_test_split(
            x, y, train_size=0.85, random_state=42
        )

        # mark categorical features
        cate_features_index = np.where(x_train.dtypes != float)[0]

        self.model.fit(
            x_train,
            y_train,
            cat_features=cate_features_index,
            eval_set=(x_test, y_test),
        )

        # Cross validating the model (5-fold)
        cv_data = cv(
            Pool(x, y, cat_features=cate_features_index),
            self.model.get_params(),
            fold_count=5,
        )

    @frogml.api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        df = df.drop([""PassengerId""], axis=1)
        return pd.DataFrame(
            self.model.predict_proba(df)[:, 1], columns=[""Survived_Probability""]
        )
```

### Training the Model

Let's create a new model instance and run the build method to train it.

**titanic/run_build.py**

```python
from titanic.main import TitanicSurvivalPrediction

## Create a new model instance
model = TitanicSurvivalPrediction()

## Run the build function which trains the model
model.build()
```

**Output**

```
Learning rate set to 0.029583
0:	learn: 0.6756870	test: 0.6751626	best: 0.6751626 (0)	total: 66.5ms	remaining: 1m 6s
1:	learn: 0.6578988	test: 0.6579213	best: 0.6579213 (1)	total: 69.8ms	remaining: 34.8s
2:	learn: 0.6427410	test: 0.6427901	best: 0.6427901 (2)	total: 72.5ms	remaining: 24.1s
```

### Registering the Trained Model

Now that we have trained a model locally, we want to register this model version and save in the in FrogML model register as a new build, so we can later deploy it to production.

The code below will register a new build under the `titanic_survival_prediction` model, with the trained titanic model we just created and a tag: `prebuilt`

**titanic/run_build.py**

```python
from frogml.sdk.frogml_client.client import FrogMLClient
from frogml.sdk.model.tools import run_local

## Creating an instance of the Frogml client
client = FrogMLClient()

## Triggering a build with model files from the local `main` directory
client.build_model(
  model_id='titanic_survival_prediction',
  prebuilt_frogml_model=model,  ## Providing a trained instance to skip remote build
  tags=['prebuilt']
)
```

**Output**

```shell
Fetching model code - Using given build ID - 116a6385-8bbf-41bb-b30f-d6528869fac9
Fetching model code - Found dependency type: PIP by file: main/requirements.txt
Fetching model code - Successfully fetched model code
Registering frogml build -  10%
Registering frogml build -  20%
Registering frogml build -  30%
Registering frogml build -  40%
Registering frogml build -  48%
Registering frogml build -  50%
Registering frogml build -  60%
Registering frogml build -  70%
Registering frogml build -  80%
Registering frogml build -  90%
Registering frogml build -  96%
Registering frogml build -  96%
Registering frogml build - 100%
Registering frogml build - Start remote build - 116a6385-8bbf-41bb-b30f-d6528869fac9
Registering frogml build - Remote build started successfully

Build ID 116a6385-8bbf-41bb-b30f-d6528869fac9 was triggered remotely
To follow build logs using frogml platform:
https://mydemo.jfrog.io/ui/ml/models/credit_risk_frogml/build/116a6385-8bbf-41bb-b30f-d6528869fac9
```

### Build SDK Configuration

The Build SDK supports a multitude of parameters which users may configure

| Description                       | Required | Default Value | Description                                                                                                       |
| :-------------------------------- | :------- | :------------ | :---------------------------------------------------------------------------------------------------------------- |
| `model_id`                        | Yes      |               | Model ID on the JFrog platform                                                                                    |
| `main_module_path`                | No       | "main"        | Path to the local folder where model files exists                                                                 |
| `dependencies_file`               | No       |               | Path to a Python dependencies file, in pip, poetry or conda format.                                               |
| `dependencies_list`               | No       |               | List of strict Python dependencies                                                                                |
| `tags`                            | No       |               | List of tags saved on the remote model build                                                                      |
| `instance`                        | No       | "small"       | Instance type during mode build                                                                                   |
| `gpu_compatible`                  | No       |               | Build the model using a GPU compatible image                                                                      |
| `run_tests`                       | No       | True          | Run tests during model build                                                                                      |
| `validate_build_artifact`         | No       | True          | Validate model deployment during build phase                                                                      |
| `validate_build_artifact_timeout` | No       |               | Model validation timeout                                                                                          |
| `frogml_model`                    | No       |               | Providing a prebuilt FrogmlModel instance will skip the build phase and use a pre-existing trained model version. |

The example below uses the advanced features of the Build SDK.

The code snippet uses a medium instance to build the model, provide build tags and build a GPU compatible image.

`Python`

```python
from frogml.sdk.frogml_client.client import FrogMLClient
from frogml.sdk.model.tools import run_local

from main import TitanicSurvivalPrediction

model = TitanicSurvivalPrediction()
model.build()

## Creating an instance of the Frogml client
client = FrogMLClient()

client.build_model(
  model_id='titanic_survival_prediction',
  main_module_path='main',
  dependencies_file="requirements.txt",
  prebuilt_frogml_model=model,
  tags=['prebuilt', 'local'],
  instance="medium",
  gpu_compatible=True
)
```

### Unsupported Parameters in Build SDK

The Build SDK supports most of the parameters that are supported in the FrogML CLI under `frogml models build`.

The following parameters are not supported:

| Parameter                | Description                                                                                                            |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `environment`            | JFrog ML environment                                                                                                   |
| `purchase-option`        | Receiving only the build id and any exception as return values (Depends on --programmatic in order to avoid UI output) |
| `deployment-instance`    | The instance size to automatically deploy the build after completion                                                   |
| `deploy`                 | Automatically deploy build after completion                                                                            |
| `json-logs`              | Return the live build logs as JSON                                                                                     |
| `param-list`             | Provide a list of parameters to the build                                                                              |
| `main-dir`               | Change the name of the `main` model directory                                                                          |
| `env-vars`               | Provide a list of environment variables                                                                                |
| `base-image`             | Change the base image of the model build                                                                               |
| `--cache / -no-cache`    | Use or disable docker cache                                                                                            |
| `git-credentials`        | Provide git credentials token                                                                                          |
| `git-credentials-secret` | The git credentials secret                                                                                             |
| `git-branch`             | Use a different git branch                                                                                             |
