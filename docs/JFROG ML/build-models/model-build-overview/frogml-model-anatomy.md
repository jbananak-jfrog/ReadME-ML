---
title: FrogML Model Anatomy
deprecated: false
hidden: false
metadata:
  robots: index
---
Here, the well-known Iris Classifier SVM model is used as an example. This model typically looks as follows:

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

## Model Directory Structure

The code below shows the default and recommended structure of a model project on JFrog ML.

```yaml Project Structure
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

## Generating the Directory Structure

Start by generating the directory structure for a JFrog ML-based model. To do so, you can use the following command:

```shell
frogml models init \ --model-directory <model-dir-path> \ --model-class-name <model-class> \ <dest>
```

Where:

* `<model-dir-path>`: Model directory name.
* `<model-class>`: JFrog ML-based model class name (Camel case)
* `<dest>`: Destination path on local host.

As an example, use the following command:

```shell
frogml models init \
    --model-directory iris_model \
    --model-class-name IrisClassifier \
    ~/
```

This creates a new directory named `iris_model` in your (the user's) home directory.

### `main` Directory

`main` is the most important directory of a JFrog ML project. Everything that is supposed to be part of the model artifact should be located in it.

### `FrogMlModel` Class

The first step is creating a model class, which defines the two mandatory functions:

1. `build` - defines the model training / loading logic, invoked on build time.
2. `predict` - defines the serving logic, invoked on every inference request.

And two optional ones:

1. `schema` - defines the model interface - input and the output of your model.
2. `initialize_model` - invoked when the model is loaded during the serving container initialization.

Read more about JFrogMl's model class method and how they can be used in the section [The FrogML Model Class](/docs/the-frogml-model-class).

For example, we can implement the Iris classifier in the following way:

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

The `main` directory should be a valid Python module, meaning it should include a `__init__.py` file.

The `__init__.py` file lets the Python interpreter know that a directory contains code for a Python module. This file should set up the imports for the FrogML model class, so it will be picked up by the model build process - in one of two ways:

**init**.py (Option 1)

```python
from .model import IrisClassifier
```

Or:

**init**.py (Option 2)

```python
from .model import IrisClassifier

def load_model():
    return IrisClassifier()
```

The `load_model` function gives more control over how the model class should be initialized during the build process.

## Dependency Files

Most projects depend on external packages to build and run correctly. JFrog ML downloads and links the dependencies on build time - based on a Python virtual environment which is used both for build and serving contexts.

JFrog ML supports the following types of dependency descriptors. Pick one! Do not include multiple dependency configuration files at once.

<Callout icon="📘" theme="info">
  **Note**

  _**FrogML SDK automatic dependency**_

  Note that the `frogml-sdk` is automatically injected as a dependency during the build process, so you do not need to manually add it to your dependency file.
</Callout>

### Conda

The `conda.yml` or `conda.yaml` should be stored in the `main` directory. For example:

conda.yaml

```
name: iris-calssifier
channels:
  - defaults
  - conda-forge
dependencies:
- python=3.10
  - pip=20.0.3
  - scikit-learn=1.0.1
```

### pip

Store the `requirements.txt` in the `main` directory:

```
scikit-learn==1.0.1
```

### Poetry

In this case, the `main` directory should contain the `pyproject.toml` file:

pyproject.toml

```toml
[tool.poetry]
name = "iris-classifier"
version = "0.1.0"
description = ""
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.10"
scikit-learn = "1.0.1"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

## Packages with JFrog-compatible Models

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

```python
from importfrogmlmodelfrompackage.model import TestModel

def load_model():
    return TestModel
```

**`tests` Directory**

The directory `tests` is where tests of each component in the model reside.

## Unit Testing

This Python file will define unit tests that will run during the build process. For example, if we will define a helper function in `<model-dir>/main/util.py` as follows:

util.py

```python
def add(x, y):
    return x + y
```

Then we can define the following test:

test_util.py

```python
from main.util import add

def add_test():
    assert add(3,2) == 5
```

## Integration Testing

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

<Callout icon="📘" theme="info">
  **Notes:**

  * **Files operations** - The current working directory of build execution is the root directory of the model. For example, if a file is located in `./main/sample.txt` and you want to read it simply open it in path `./main/sample.txt` where `.` represents the model root directory.
  * **Model fields** - Model fields should be objects which can be pickled, S3 client, for example, can't pickle due to the fact that the session should remain active.
</Callout>

## Additional Directories

By default, the FrogML-SDK does not copy any other directories from the build directory, only `main` and `tests`.

However, it is possible to modify this behavior by adding the `--dependency_required_folders` parameter to the model build command.

Examples:

```shell
frogml models build --model-id your_model_id --dependency_required_folders additional_dir .
frogml models build --model-id your_model_id --dependency_required_folders additional_dir --dependency_required_folders some_other_dir .
```

Within the build container, additional dependency folders are placed alongside `main` and `tests` in the `/frogml/model_dir/`.

Here's an illustrative directory structure:

```yaml Project Files
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
