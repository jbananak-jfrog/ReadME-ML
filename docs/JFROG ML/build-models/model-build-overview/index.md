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

The build process can be triggered from the FrogML CLI/SDK or the platform UI, and it runs on JFrog’s scalable infrastructure that supports any workload size, from lightweight pre-trained models to full training pipelines.

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
  **Important**

  _Please install the FrogML Python SDK._
</Callout>

### 1. Creating a New Model

Start by creating a new project and model on the JFrog ML platform. Note that the command doesn't generate local output but rather creates a remote project and model.

Your model ID will be the model name in lowercase letters and stripped from spaces, in this example, `titanic`.

```
frogml models create "Titanic" --project "example-models"
```

### 2. Generating the Model Code

Generate the **Titanic** example model, which is available in the example templates provided with the FrogML SDK.

This command will create the files needed to build a model on JFrog ML.

```
frogml models init --example titanic .
```

The models init command works in the following format:

```
frog models init --example <example-name> <local-model-directory>
```

### 3. Building Your Model

With the local model code, and our new model on JFrog ML, we can initiate a model build. Build names are unique across a project.

<Callout icon="📘" theme="info">
  **Note**

  Note that the build name parameter is optional.
</Callout>

```
frogml models build --model-id titanic --name v1 ./titanic_survival_classification
```

The models build command works in the following format:

```
frogml models build --model-id <remote-model-id> --name <build-name> <local-model-directory>
```

Running the above command generates the build ID and a link you can follow to view the live build status:

```
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

```
frogml models builds logs -b <build-id> --follow
```

* `<build-id>` - The build ID that you received when executing the build remotely.

### Building Models with GPUs

JFrog ML [_GPU Instances_](/docs/instance-sizes-ml-credits) provide high-performance computing resources that can significantly accelerate the model build process. Easily customize your build resources to achieve faster training times and better results.

To build a model on a GPU instance, specify the following additional arguments in the model build command:

```
frogml models build  --model-id <model-id> --instance gpu.t4.xl .
```

<Callout icon="📘" theme="info">
  **Note**

  _**Choosing the Correct GPU**_

  Visit the JFrog ML [_GPU Resources_](/docs/instance-sizes-ml-credits) page to select the resources that best fit your use-case.

  Each GPU type has its own configuration of pre-defined memory and number of CPUs.
</Callout>

<Callout icon="📘" theme="info">
  **Note**

  JFrog ML utilizes EC2 Spot instances for cost-effective GPU-based builds. This may result in a slightly extended wait time when initiating GPU Spot instances.
</Callout>

### Deploying Models with GPUs

<Callout icon="📘" theme="info">
  **Note**

  To deploy models using a GPU, you do not need to build it on a GPU instance.

  Simply use the `--gpu-compatible` flag during the model build process.
</Callout>

When deploying a model on a GPU instance, you must verify that the model was build using a GPU compatible image. Building a model using a GPU compatible image installs additional dependencies and drivers.

Creating a GPU compatible image is simply done by adding the `--gpu-compatible` flag:

```
frogml models build  --model-id <model-id> --gpu-compatible .
```

Running the above command will build your model on a regular CPU instance, but will enable you to later deploy it on a GPU instance.

### Tagging Your Model Build

Tags can be attached to specific builds for identification and tracking.

Add model tags from JFrog ML UI manually, or add tags via the FrogML CLI:

```
frogml models build --model-id <model-id> -T <tag_1> -T <tag_2> <local-model-directory>
```

Use the `model-id` of the model to which you want to attach tags.

### Using Environment Variable in Model Builds

You may use and pass environment variables to your models build in the CLI using the following command:

```
frogml models build --model-id <model-id> -E ENV_VAR=VALUE <local-model-directory>
```

or for example with mock values:

```
frogml models build --model-id "titanic" -E VERSION_NUMBER=1.2 -E MODEL_NAME=catboost .
```

## Model Builds SDK

Data scientists often train models in Workspaces, Jupyter notebooks or locally, and require a seamless process to save, register and manage model versions for production use.

JFrog ML provides the Build Model SDK to address this need, simplifying the transition from model training to deployment, from your local machine or your Jupyter notebook.

### Key Features

##### 1. Build Models from Workspaces

JFrog ML Build SDK simplifies the registration of locally or Jupyter notebook-based model training. It ensures precise versioning, tracking, and effortless transition from research to deployment in production.

##### 2. Python-driven Model Builds

Automate model builds using Python and seamlessly integrate with continuous integration/continuous deployment (CI/CD) pipelines and various automation workflows.

##### 3. Streamlined Versioning for Pre-built Models

Build and register pre-trained models from any Python environment by supplying existing trained model instances, skipping remote build phases.

#### Using Build SDK

This document will explore the various options available when using the Build SDK.

In general, there are two choices when working with the Build SDK:

1. Providing a pre-trained model artifact to the `build_model` method.
2. Omitting a pre-built model, in which case the SDK will upload local model files and builds the model on JFrog ML.

```
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

#### Folder Structure

<Callout icon="⚠️" theme="warning">
  **Warning**

  _**File Structure Requirements**_

  When using the Build SDK, your file and folder structure is preserved when uploading to JFrogML.

  Please ensure to:

  * Avoid Python files with top-level executable statements in the build directory.
  * All code with side effects should be guarded with `if __name__ == "__main__"` blocks.
  * Place shared functionality in properly encapsulated classes and functions.

  Failure to follow these guidelines may cause unintended code execution during the import process.
</Callout>

The Build SDK uploads local model files together with the trained model object. By default, the Build SDK uploads the `main` folder under the current file location.

Make sure to place your model files in the `main` directory.

```
-> your-model-directory
---- build_sdk_runner.py
---> main
------ model.py
```

It is possible to change the uploaded directory by providing an explicit path as will be described in this document.

#### Building Pre-trained Models

Use the Build SDK to build models with an existing instance of a trained model to upload the pre-trained model artifact. This flexibility empowers data scientists to train or fine-tune models within notebooks, effortlessly incorporate the trained versions into the model registry, and deploy them to production environments.

#### Creating a Model Instance

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

###### requirements.txt

```
pandas
scikit-learn
catboost
```

###### titanic/main/**init**.py

```python
from .model import TitanicSurvivalPrediction

def load_model():
    return TitanicSurvivalPrediction()
```

###### titanic/main/model.py

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

#### Training the Model

Let's create a new model instance and run the build method to train it.

**titanic/run_build.py**

```
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

#### Registering the Trained Model

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

```
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

#### Build SDK configuration

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

For example, the below is an example using the advanced features of the Build SDK.

The code snippet using a medium instance to build the model, provide build tags and build a GPU compatible image.

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

#### Unsupported Parameters in Build SDK

The Build SDK support most of the parameters that are supported in the FrogML CLI under `frogml models build`

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

## Managing Dependencies

JFrog ML supports a variety of Python frameworks to manage model dependencies.

### Supported Python Versions

When building and managing your Python projects, different tools have varying levels of support for Python versions. Below is a summary of the supported Python versions for each tool:

* **Poetry** supports Python versions: 3.8 - 3.11
* **Conda** supports Python versions: 3.8 - 3.11
* **requirements.txt (pip)** supports only Python 3.9

### Using Poetry with JFrogML

<Callout icon="⚠️" theme="warning">
  **Warning**

  JFrogML uses **Poetry version 1.8.3**.JFrog ML supports `poetry.lock` files as long as they're under the same scope as the `pyproject.toml` file.
</Callout>

#### Model Directory Structure

```
frogml_based_model/
├── main/
├──── pyproject.toml
├──── poetry.lock
├── tests/
```

Both files `pyproject.toml` and `poetry.lock` will be used by Poetry while executing the `poetry install` command.

#### Example Project Setup

pyproject.toml

```toml
[tool.poetry]
name = "example-project"
version = "0.1.0"
description = "Example project for production and development"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = ">=3.11,<3.12"
scipy = "^1.7"
scikit-learn = "^0.24"
catboost = "^1.0"

[tool.poetry.dev-dependencies]
frogml-cli = "*"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

> The `frogml-sdk` dependency is included only in the `dev` section, as it's needed for local development but not for remote builds. When you run the `frogml models build` command, the SDK version you used locally will be automatically included in the remote environment.

### Using Conda with JFrogML

> JFrogML uses **Conda version 24.7.1**

#### Model Directory Structure

```
frogml_based_model/
├── main/
├──── conda.yml
├──── ...
├── tests/
```

The `conda.yml` file can be placed at the root level alongside `main` or within it—both structures work equally well.

#### Example Project Setup

To get started, here’s a basic `conda.yml` setup:

conda.yml

```conda
name: example_conda_model
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.11
  - scipy
  - scikit-learn
  - catboost
  - pip:
      - # additional pip dependencies
```

<Callout icon="📘" theme="info">
  **Note**

  There’s no need to manually add `frogml-sdk` to the environment. JFrogML’s build process includes it automatically based on your local version.
</Callout>

### .frogmlignore file

Occasionally, we may want to exclude a file from the JFrog ML build but keep it in the repository with the model code. In such cases, we should add the `.frogmlignore` file to the root directory of our project.

In the file, we define the patterns to match files to exclude from the model build. For example, suppose we have the following file structure:

```
.frogmlignore
main/
    __init__.py
    model.py
    README.md
tests/
    test_model.py
research/
    paper_a.pdf
    paper_b.pdf
```

If we want to exclude the entire `research` directory and the `README.md` file from the build, our `.frogmlignore` file may contain:

```
research
README.md
```

<Callout icon="📘" theme="info">
  **Note**

  _**Hidden Files**_

  By default, JFrog ML disregards hidden files. Hidden files are files or directories whose names start with a dot (`.`) in Unix-like operating systems, or they may have the "Hidden" attribute set in Windows. These files are typically used to store configuration data or hold temporary information.

  Suppose you have a directory with files and subdirectories, including a hidden file named `.config_file`. JFrog ML, following its default behavior, will exclude this file from processing when triggering a remote build.
</Callout>

### Incorporating Python Dependencies from .whl Files

JFrog ML facilitates the use of Python dependencies packaged as `.whl` files through `requirements.txt` and `conda.yaml` for managing dependencies. It is important to note that Poetry's support for dependencies from .whl files is limited.

#### 1. Preparing Your .whl Files

First, ensure your `.whl` file(s) are either uploaded with your model code or fetched from external storage. For instructions on uploading additional dependencies, refer to the FrogML CLI documentation (`frogml models build --help`). Below is an example directory structure for your model, where `main` is uploaded by default and the `dep` directory, containing the pandas dependency in a `.whl` file, is included via the `--dependency-required-folders dep` option in the FrogML command.

The wheel file has to be uploaded as part of an additional dependencies folder, and not as part of the main model folder.

Model Build Container Directory

```
/frogml/model_dir/
.├── main                   # Main directory containing core code
│   ├── __init__.py        # An empty file that indicates this directory is a Python package
│   ├── model.py           # Defines the Credit Risk Model
│   └── conda.yaml         # Conda environment configuration
├── dep                    # Additional dependency directory added with --dependency-required-folders
│   └── pandas-2.2.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
├── tests                  # Empty directory reserved for future test
│   └── ...                # Future tests
└── 
```

#### 2. Configuring Dependency Management Files

##### Conda

Include the `.whl` file in your `conda.yaml` as follows:

conda.yaml

```
name: test_model
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.9
  - pip:
    - "/frogml/model_dir/dep/pandas-2.2.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
```

##### Poetry

pyproject.toml

```toml
[tool.poetry]
name = "example-project"
version = "0.1.0"
description = "Example project for production and development"
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.9"
scipy = "^1.7"
scikit-learn = "^0.24"
catboost = "^1.0"
pandas = { path = "dep/pandas-2.2.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl" }

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

##### Requirements.txt

Directly reference the `.whl` file path relative to the requirements file location:

`requirements.txt`

```
## requirements file located in main model folder
./../deps/wheel_test-0.1-py3-none-any.whl

## requirements file located in model dir
./deps/wheel_test-0.1-py3-none-any.whl
```

#### 3. Using the Dependency in Your Code

Once the dependency is properly configured, you can import and use it in your Python code as usual:

`Python`

```python
import pandas as pd
```

## FrogML Model Anatomy

As an example, we will use the well-known Iris Classifier SVM model, which typically looks as follows:

train.py

```python
from sklearn import svm
from sklearn import datasets

## Load training data
iris = datasets.load_iris()
X, y = iris.data, iris.target

## Model Training
clf = svm.SVC(gamma='scale')
clf.fit(X, y)
```

### Model Directory Structure

Below, we show the default and recommended structure of a model project on JFrog ML.

```python
frogml_based_model/
├── main/
│   ├── __init__.py    # Required for exporting model from main
│   ├── model.py       # FrogML model definition.
│   ├── util.py        # Represents any helper/util python modules.
│   ├── <dependecies>  # Dependencies file - Poetry/PyPI/Conda
├── tests/
│   ├── ut/            # Unit tests
│   │   ├── sample_test.py
│   ├── it/            # Integration tests
│   │   ├── sample_test.py
```

### Generating the Directory Structure

First, we will generate the directory structure for our JFrog ML-based model. To do so, you can use the following command:

```
frogml models init \ --model-directory <model-dir-path> \ --model-class-name <model-class> \ <dest>
```

Where:

* `<model-dir-path>`: Model directory name.
* `<model-class>`: JFrog ML-based model class name (Camel case)
* `<dest>`: Destination path on local host.

As an example, we will use the following command:

```
frogml models init \
    --model-directory iris_model \
    --model-class-name IrisClassifier \
    ~/
```

That will create a new directory named `iris_model` at the user's home directory.

#### `main` Directory

`main` is the most important directory of a JFrog ML project. Everything that is supposed to be part of the model artifact should be located in it.

#### `FrogMlModel` Class

The first step is creating a model class, which defines the two mandatory functions:

1. `build` - defines the model training / loading logic, invoked on build time.
2. `predict` - defines the serving logic, invoked on every inference request.

And two optional ones:

1. `schema` - defines the model interface - input and the output of your model.
2. `initialize_model` - invoked when the model is loaded during the serving container initialization.

Read more about JFrogMl's model class method and how they can be used in the dedicated [section](/docs/model-build-overview/the-frogml-model-class).

For example, we can implement the Iris classifier in the following way:

```
import pandas as pd
from sklearn import svm, datasets
from frogml import api,FrogMlModel
from frogml.sdk.model.schema import ExplicitFeature, InferenceOutput, ModelSchema

class IrisClassifier(FrogMlModel):
    def __init__(self):
        self._gamma = 'scale'
        self._model = None

    def build(self):
        iris = datasets.load_iris()
        X, y = iris.data, iris.target

        clf = svm.SVC(gamma=self._gamma)
        self._model = clf.fit(X, y)

    @api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        return pd.DataFrame(data=self._model.predict(df), columns=['species'])

    def schema(self):
        return ModelSchema(
            inputs=[
                ExplicitFeature(name="sepal_length", type=float),
                ExplicitFeature(name="sepal_width", type=float),
                ExplicitFeature(name="petal_length", type=float),
                ExplicitFeature(name="petal_width", type=float)
            ],
            outputs=[
                InferenceOutput(name="species", type=str)
            ])
```

The `main` directory should be a valid Python module, meaning it should include a `__init__.py` file.

The `__init__.py` file lets the Python interpreter know that a directory contains code for a Python module. This file should set up the imports for the FrogML model class, so it will be picked up by the model build process - in one of two ways:

**init**.py (Option 1)

```
from .model import IrisClassifier
```

Or:

**init**.py (Option 2)

```
from .model import IrisClassifier

def load_model():
    return IrisClassifier()
```

The `load_model` function gives more control over how the model class should be initialized during the build process.

### Dependency Files

Most projects depend on external packages to build and run correctly. JFrog ML downloads and links the dependencies on build time - based on a Python virtual environment which is used both for build and serving contexts.

JFrog ML supports the following types of dependency descriptors. Pick one! Do not include multiple dependency configuration files at once.

<Callout icon="📘" theme="info">
  **Note**

  _**FrogML SDK automatic dependency**_

  Note that the `frogml-sdk` is automatically injected as a dependency during the build process, so you do not need to manually add it to your dependency file.
</Callout>

#### Conda

The `conda.yml` or `conda.yaml` should be stored in the `main` directory. For example:

conda.yaml

```
name: iris-calssifier
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.8
  - pip=20.0.3
  - scikit-learn=1.0.1
```

#### pip

Store the `requirements.txt` in the `main` directory:

```
scikit-learn==1.0.1
```

#### Poetry

In this case, the `main` directory should contain the `pyproject.toml` file:

pyproject.toml

```toml
[tool.poetry]
name = "iris-classifier"
version = "0.1.0"
description = ""
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.8"
scikit-learn = "1.0.1"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

### Packages with JFrog-compatible Models

It's also possible to store a JFrog-compatible model in a Python package and add it as a dependency to the FrogML model. We have to implement the model class and build it as a Python package. Note that our package needs `frogml-sdk` as its dependency.

In our example, we assume that we have created a package called `importfrogmlmodelfrompackage` that contains a submodule `model` with the `TestModel` class. The `TestModel` class implements the `FrogMlModel`.

Later, when we configure the FrogML model:

1. Create a new FrogML model using the `frogml models init` command.
2. Add the package to the dependencies.

For example, if we use pip to manage dependencies and the whl file is stored in a GitHub repository, we must add the following line to our `requirements.txt` file:

```
https://raw.githubusercontent.com/[github_user]/[github_repository_name]/[branch_name]/[path_to_directory]/[whl_file_name]#egg=[package_name]
```

If we use a different package manager, we should follow their instructions regarding adding whl files or private package repositories as dependencies.

1. We must remove the content of the `__init__.py` file.
2. In the `model.py` file, we import the FrogML model class (the one added as a dependency in pip) and implement the `load_model` function to return the **class** (not an instance of the class!). For example:

```
from importfrogmlmodelfrompackage.model import TestModel

def load_model():
    return TestModel
```

#### `tests` Directory

The directory `tests` is where tests of each component in the model reside.

### Unit Testing

This Python file will define unit tests that will run during the build process. For example, if we will define a helper function in `<model-dir>/main/util.py` as follows:

util.py

```
def add(x, y):
    return x + y
```

Then we can define the following test:

test_util.py

```
from main.util import add

def add_test():
    assert add(3,2) == 5
```

### Integration Testing

This Python file will define integration tests we will run during the build process after the serving container is built and initialized.

During the integration tests, a real model deployment will be running, and you will be able to perform inference using pytest fixture, which will be auto-configured with a client that can be invoked against the model currently being built.

For example:

test_model.py

```python
import pandas as pd
from frogml.core.testing.fixtures import real_time_client

def test_iris_classifier(real_time_client):
    feature_vector = [[5.1, 3.5, 1.4, 0.2]]
    iris_type = real_time_client.predict(feature_vector)
    assert iris_type == 1
```

### Notes

* **Files operations** - The current working directory of build execution is the root directory of the model. For example, if a file is located in `./main/sample.txt` and you want to read it simply open it in path `./main/sample.txt` where `.` represents the model root directory.
* **Model fields** - Model fields should be objects which can be pickled, S3 client, for example, can't pickle due to the fact that the session should remain active.

### Additional Directories

By default, the FrogML-SDK does not copy any other directories from the build directory, only `main` and `tests`.

However, it is possible to modify this behavior by adding the `--dependency_required_folders` parameter to the model build command.

Examples:

```
frogml models build --model-id your_model_id --dependency_required_folders additional_dir .
frogml models build --model-id your_model_id --dependency_required_folders additional_dir --dependency_required_folders some_other_dir .
```

Within the build container, additional dependency folders are placed alongside `main` and `tests` in the `/frogml/model_dir/`.

Here's an illustrative directory structure:

```
/frogml/model_dir/
.
├── main                     # Main directory containing core code
│   ├── __init__.py
│   ├── model.py
│   └── requirements.txt
│
├── additional_dir           # Additional dependency directory added with --dependency-required-folders
│   └── files..
│
├── tests
│   └── ...
└──
```

For smooth integration, ensure that your additional folders are placed in the same directory with `main`. When specifying these directories in your build command, use their names directly, omitting any `./` prefix.

Should your additional dependency folder not be located within the current working directory, it's necessary to specify its absolute path. However, this will lead to the recreation of the specified path hierarchy within the build container, so plan accordingly to maintain the desired structure.

## The FrogML Model Class

### The`FrogMlModel`

The FrogML model class is the core abstraction which encapsulates the model build and serving logic. Every Frogml-based model should inherit from `FrogMModel` which is defined as:

```python
from frogml import FrogMlModel

class BaseModel:
    """
    Base class for all Frogml based models.
    """

    def build(self):
        """
        Responsible for loading the model. This method is invoked during build time (frogml build command)

        Example usage:

        >>> def build(self):
        >>>     ...
        >>>     train_pool = Pool(X_train, y_train, cat_features=categorical_features_indices)
        >>>     validate_pool = Pool(X_validation, y_validation, cat_features=categorical_features_indices)
        >>>     self.catboost.fit(train_pool, eval_set=validate_pool)

        :return:
        """
        self.fit()

    def predict(self, df):
        """
        Invoked on every API inference request.
        :param df: the inference vector

        Example usage:

        >>> def predict(self, df) -> pd.DataFrame:
        >>>     return pd.DataFrame(self.catboost.predict(df), columns=['churn'])

        :return: model output (inference results), as a pandas dataframe
        """
        pass

    def schema(self) -> ModelSchema:
        """
        Specification of the model inputs and outputs. Optional method

        Example usage:

        >>> from frogml.sdk.model.schema import ModelSchema, Prediction, ExplicitFeature
        >>>
        >>> def schema(self) -> ModelSchema:
        >>>     model_schema = ModelSchema(
        >>>     inputs=[
        >>>         RequestInput(name="State", type=str),
        >>>     ],
        >>>     outputs=[
        >>>         InferenceOutput(name="score", type=float)
        >>>     ])
        >>>     return model_schema

        :return: a model schema specification
        """
        pass

    def initialize_model(self):
        """
        Invoked when a model is loaded at serving time. Called once per model instance initialization. Can be used for
        loading and storing values that should only be available in a serving setting.
        """
        pass
```

For reference, a complete working example of a FrogML model, in the `model.py` file:

```python
import pandas as pd
from sklearn import svm, datasets
from frogml import api,FrogMlModel
from frogml.sdk.model.schema import ExplicitFeature, InferenceOutput, ModelSchema

class IrisClassifier(FrogMlModel):
    def __init__(self):
        self._gamma = 'scale'
        self._model = None

    def build(self):
        iris = datasets.load_iris()
        X, y = iris.data, iris.target

        clf = svm.SVC(gamma=self._gamma)
        self._model = clf.fit(X, y)

    @api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        return pd.DataFrame(data=self._model.predict(df), columns=['species'])

    def schema(self):
        return ModelSchema(
            inputs=[
                ExplicitFeature(name="sepal_length", type=float),
                ExplicitFeature(name="sepal_width", type=float),
                ExplicitFeature(name="petal_length", type=float),
                ExplicitFeature(name="petal_width", type=float)
            ],
            outputs=[
                InferenceOutput(name="species", type=str)
            ])
```

Let's break it down:

### Build

The `build` method defines the model training logic and is invoked once, on build time. In case the model should be trained on the FrogML platform - the function should include the training code invocation:

```
def build(self):
    iris = datasets.load_iris()
    X, y = iris.data, iris.target

    clf = svm.SVC(gamma=self._gamma)
    self._model = clf.fit(X, y)
```

### Predict

The `predict` method defines the serving logic, invoked on every prediction request:

```
@api()
def predict(self, df: pd.DataFrame) -> pd.DataFrame:
    return pd.DataFrame(data=self._model.predict(df), columns=['species'])
```

Notice that in this case we use pandas `DataFrame` for both the input and the output. For other options, see <Anchor label="Prediction Input & Output Adapters" title="Prediction Input & Output Adapters" href="/docs/prediction-input---output-adapters">Prediction Input & Output Adapters</Anchor>.

<Callout icon="📘" theme="info">
  **Note**

  **Inference Batching**

  By default, the endpoint doesn't batch predictions. You can control this configuration using the `MAX BATCH SIZE` parameter (default: 1).

  If you enable batching, your endpoint code must be ready to handle multiple model invocations during a single call to the `predict` method.
</Callout>

### `@api` Decorator

JFrogML's API decorator adds additional functionality to the `predict` method. There are currently 4 options:

| Paramater            | Type                | Description                                                                                                                                                                           | Default Value            |
| :------------------- | :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------- |
| `analytics`          | `bool`              | Whether to activate JFrogML's built-in inference data collection mechanism, which streams all inference requests to the [FrogML Lake](/docs/inference-analytics).                     | `True`                   |
| `feature_extraction` | `bool`              | Whether to activate the automatic feature extraction mechanism, pulling features from JFrog ML's feature store. For more info see  [Features in Inference](doc:feature-consumption) . | `False`                  |
| `Input Adapter`      | `BaseInputAdapter`  | To which format should the input request be serialized. For a list of supported adapters see [Input & Output Adapters](/docs/prediction-input---output-adapters)  .                   | `DataframeInputAdapter`  |
| `Output Adapter`     | `BaseOutputAdapter` | To which format should the output of the `predict` function be serialized. For a list of supported adapters see [Input & Output Adapters](/docs/prediction-input---output-adapters).  | `DataframeOutputAdapter` |

### Schema

The optional schema method defines the input and output schemas of your model:

```python
def schema(self):
    from frogml.sdk.model.schema import ModelSchema, InferenceOutput
    from frogml.sdk.model.schema_entities import RequestInput
    
    return ModelSchema(
        inputs=[
            RequestInput(name="sepal_length", type=float),
            RequestInput(name="sepal_width", type=float),
            RequestInput(name="petal_length", type=float),
            RequestInput(name="petal_width", type=float),
        ],
        outputs=[
            InferenceOutput(name="species", type=str)
        ])
```

It is used for two main purposes:

1. For creating inference templates that make it easier for model consumers to integrate with the model. There can be found under the **Interface** tab in the management platform model page.
2. As an integration point with JFrogML's Feature store, for feature auto extraction during inference time. For more info see [Features in Inference](doc:feature-consumption).

After building and deploying the model, the schema information will appear in the interface tab of the model, with a snippet of code used for interactions with the model.

### Initialize model

The `initalize_model` is invoked when the model is loaded during the serving container initialization. It can be used to execute logic that should be applied once and only in a production setting (meaning not in build time).

For example, loading secrets:

```python
import boto3
import frogml
from frogml.core.clients.secret_service import SecretServiceClient

def initialize_model(self):
    secret_service = SecretServiceClient()
    aws_api_key = secret_service.get_secret('aws_api_key')
    aws_secret_key = secret_service.get_secret('aws_secret_key')
    aws_region = secret_service.get_secret('aws_region')

@frogml.api()       
def predict(self, df):
    boto3.client(
         's3',
         aws_access_key_id=aws_api_key,
         aws_secret_access_key=aws_secret_key
         region_name=aws_region)...
```

Or even loading a pre-trained model:

```
def initialize_model(self):
    with open('model.pkl', 'rb') as infile:
        self._model = pickle.load(infile)

@api()
def predict(self, df: pd.DataFrame) -> pd.DataFrame:
    return pd.DataFrame(data=self._model.predict(df), columns=['species'])
```

### Accessing the FrogML Logger

To log statements during the build and deployment stages on the FrogML platform, you can utilize the FrogML `Logger` object. This is accessible through the utility method demonstrated below:

```
from frogml.core.tools.logger.logger import get_frogml_logger

logger = get_frogml_logger()


class MyModel(FrogMlModel):
        def init():
    ...

        def build():
          ...
    
    logger.info("your message here")

...
```

This approach allows for the integration of logging directly into your model's lifecycle, facilitating training and inference insights and diagnostics. Your model logs are available in the **Model Builds** -> **Logs** and under **Deployments** -> **Runtime Logs**.
