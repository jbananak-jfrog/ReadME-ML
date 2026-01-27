---
title: Model Build Overview
deprecated: false
hidden: false
metadata:
  title: Model Build Overview
  description: Learn how to easily build a model on JFrog ML
  legacyUUIDs:
    - UUID-1e1717bd-a990-ef61-f73b-c88807281bc6
    - UUID-b99330dc-89d0-f415-bb3b-5394dacbcbda
    - UUID-e7749bd6-96fd-3645-085d-f553d6788e1a
    - UUID-5c446e4d-6004-628e-efe6-2532736b1eea
    - UUID-92351a63-7ff2-4dea-35a1-91e706555c62
    - UUID-3d6aba73-ad31-9fb8-9fe4-124abbd7e4a6
    - UUID-4a1ddc4e-f4fa-67bf-3ced-6b6e7ad7e987
    - UUID-a0c3f5dc-d2e5-23cd-d3f7-208d80cc9d39
    - UUID-96679a3e-16ee-5f70-9b1d-1501870d98ea
    - UUID-251bbfb6-7da7-6343-d503-444ab0e79174
  robots: index
---
Learn how to easily build a model on JFrog ML

## About Model Builds

A model build in JFrog ML is the process of creating a ready-for-deployment, trained, serialized, and tested version of your machine learning model.

During the build, JFrog ML packages your model’s source code, dependencies, and metadata into a secure, immutable artifact stored in JFrog Artifactory.

The build process can be triggered from the FrogML CLI/SDK, and it runs on JFrog’s scalable infrastructure that supports any workload size, from lightweight pre-trained models to full training pipelines.

<Image alt="Model build lifecycle diagram" border={false} src="https://files.readme.io/1266aa9e944c458dfad58d56b022c7e24d1d56346550568c46cb84efd5eab7a3-uuid-0a72d21a-026a-197b-1e97-18074b5c7136.png" />

## Model Build Lifecycle

The remote build process on JFrog ML comprises the following steps:

1. Creating the model's virtual environment
2. Executing the build function
3. Running unit and integration tests
4. Serializing the model
5. Building a docker image
6. Pushing the docker image to JFrog ML model registry

## Building a Model with FrogML CLI

The following steps will show you how easily to build your first model on JFrog ML.

<Callout icon="❗️" theme="error">
  **Important** - You need to [install the FrogML CLI](/docs/setting-up-jfrog-ml#install-frogml-cli).
</Callout>

### 1. Creating a New Model

Start by creating a new project and model on the JFrog ML platform. Note that the command doesn't generate local output but rather creates a remote project and model.

Your model ID will be the model name in lowercase letters and stripped from spaces, in this example, `titanic`.

```shell
frogml models create "Titanic" --project "example-models"
```

### 2. Generating the Model Code

Generate the **Titanic** example model, which is available in the example templates provided with the FrogML SDK.

This command will create the files needed to build a model on JFrog ML.

```shell
frogml models init --example titanic .
```

The models init command works in the following format:

```shell
frogml models init --example <example-name> <local-model-directory>
```

### 3. Building Your Model

With the local model code, and our new model on JFrog ML, we can initiate a model build. Build names are unique across a project.

<Callout icon="📘" theme="info">
  **Note**

  The build name parameter is optional.
</Callout>

```shell
frogml models build --model-id titanic --name v1 ./titanic_survival_classification
```

The models build command works in the following format:

```shell
frogml models build --model-id <remote-model-id> --name <build-name> <local-model-directory>
```

Running the above command generates the build ID and a link you can follow to view the live build status:

```shell
✅ Fetching model code (0:00:00.22)
✅ Registering frogml build - 100% (0:00:05.77)

Build ID 2cac1883-47eb-44dd-9806-bdd9887dcc16 triggered remotely

########### Follow build logs in the CLI
frogml models builds logs -b 2cac1883-47eb-44dd-9806-bdd9887dcc16 --follow

########### Follow build logs in the platform
https://mydemo.jfrog.io/ui/ml/models/credit_risk/build/2cac1883-47eb-44dd-9806-bdd9887dcc16
```

### 4. Tracking Build Progress

Building a model takes several minutes to complete. To view the build status, open the the model's build table.

When the build is complete, you can proceed to deploy your model.

There are two options for viewing the build progress logs:

#### Option1: Build Page in JFrog ML UI

Visit your model's page and choose the current build ID. Alternatively, follow the link you received in the CLI.

#### Option 2: Following Logs in the CLI

```shell
frogml models builds logs -b <build-id> --follow
```

* `<build-id>` - The build ID that you received when executing the build remotely.

### Building Models with GPUs

JFrog ML [_GPU Instances_](/docs/instance-sizes-ml-credits#deploy-models-on-gpu-instances) provide high-performance computing resources that can significantly accelerate the model build process. Easily customize your build resources to achieve faster training times and better results.

To build a model on a GPU instance, specify the following additional arguments in the model build command:

```shell
frogml models build  --model-id <model-id> --instance gpu.t4.xl .
```

<Callout icon="📘" theme="info">
  **Note** - _**Choosing the Correct GPU**_

  Visit the JFrog ML [_GPU Resources_](/docs/instance-sizes-ml-credits#instances-sizes-in-the-ui) page to select the resources that best fit your use-case.

  Each GPU type has its own configuration of pre-defined memory and number of CPUs.
</Callout>

<Callout icon="📘" theme="info">
  **Note**

  JFrog ML utilizes Spot instances for cost-effective GPU-based builds. This may result in a slightly extended wait time when initiating GPU Spot instances.
</Callout>

### Deploying Models with GPUs

<Callout icon="📘" theme="info">
  **Note**

  To deploy models using a GPU, you do not need to build it on a GPU instance.

  Simply use the `--gpu-compatible` flag during the model build process.
</Callout>

When deploying a model on a GPU instance, you must verify that the model was build using a GPU compatible image. Building a model using a GPU compatible image installs additional dependencies and drivers.

Creating a GPU compatible image is simply done by adding the `--gpu-compatible` flag:

```shell
frogml models build  --model-id <model-id> --gpu-compatible .
```

Running the above command will build your model on a regular CPU instance, but will enable you to later deploy it on a GPU instance.

### Tagging Your Model Build

Tags can be attached to specific builds for identification and tracking.

Add model tags from JFrog ML UI manually, or add tags via the FrogML CLI:

```shell
frogml models build --model-id <model-id> -T <tag_1> -T <tag_2> <local-model-directory>
```

Use the `model-id` of the model to which you want to attach tags.

### Using Environment Variable in Model Builds

You may use and pass environment variables to your models build in the CLI using the following command:

```shell
frogml models build --model-id <model-id> -E ENV_VAR=VALUE <local-model-directory>
```

or for example with mock values:

```shell
frogml models build --model-id "titanic" -E VERSION_NUMBER=1.2 -E MODEL_NAME=catboost .
```

## Model Builds SDK

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
