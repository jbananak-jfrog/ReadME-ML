---
title: Model Registry and Metadata
deprecated: false
hidden: false
metadata:
  title: Model Registry and Metadata
  description: >-
    The JFrog ML platform enables you to log model metadata, artifacts, and
    DataFrames, as well as to track experiments effectively. To utilize this
    capability, integrate the utility functions described below into your
    FrogMlModel .
  legacyUUIDs:
    - UUID-c563baaa-9871-cfe3-7e26-a613e4c0ca99
    - UUID-4e4da9cf-2977-d65c-3530-ff4bf7c15f4f
  robots: index
---
The JFrog ML platform enables you to log model metadata, artifacts, and DataFrames, as well as to track experiments effectively. To utilize this capability, integrate the utility functions described below into your `FrogMlModel`.

## Log Build Metrics

When executing a build, you can choose to store the model metrics. You can log any **decimal number** as a model metric using the `log_metric` function:

```python
import frogml

frogml.log_metric({"<key>": "<value>"})
```

Alternatively, import the `log_metric` method directly:

```python
from frogml.sdk.model.model_version_tracking import log_metric

log_metric({"<key>": "<value>"})
```

## Log Training Metrics

In the example below, the model F1 score is logged:

```python
from frogml import FrogMlModel
from sklearn import svm, datasets
from sklearn.metrics import f1_score
from sklearn.model_selection import train_test_split
from frogml.sdk.model.model_version_tracking import log_metrics


class IrisClassifier(FrogMlModel):

    def __init__(self):
        self._gamma = 'scale'

    def build(self):
        # Load training data
        iris = datasets.load_iris()
        X, y = iris.data, iris.target
        X_train, X_test, y_train, y_test = train_test_split(X, y)

        # Train model
        clf = svm.SVC(gamma=self._gamma)
        self.model = clf.fit(X_train, y_train)
    
        # Store model metrics
        y_predicted = self.model.predict(X_test)
        f1 = f1_score(y_test, y_predicted)
        
        # Log metrics to JFrog ML
        log_metrics({"f1": f1})

    def predict(self, df):
        return self.model.predict(df)
```

## Log Build Parameters

When executing a build, you can log model parameters. The parameters can be logged in two ways:

Using `log_param` **- **an API which can be used from FrogML-based models:****,

```python
import frogml

frogml.log_param({"<key>": "<value>"})
```

Or, alternatively, i**mport the `log_param` method directly**:

```python
from frogml.sdk.model.model_version_tracking import log_param

log_param({"<key>": "<value>"})
```

The supported data types for logging are:

<Columns layout="auto">
  <Column>
    * `bool`
  </Column>

  <Column>
    * `object`
  </Column>

  <Column>
    * `int64`
  </Column>

<Column>
    * `float64`
  </Column>
</Columns>

<br />

<Columns layout="auto">
  
  <Column>
    * `datetime64`
  </Column>

  <Column>
    * `datetime64[ns]`
  </Column>

  <Column>
    * `datetime64[ns, UTC]`
  </Column>
</Columns>

* <br />

For example:

```python
import frogml
from frogml import FrogMlModel
from sklearn import svm, datasets
from sklearn.metrics import f1_score
from sklearn.model_selection import train_test_split
from frogml.sdk.model.model_version_tracking import log_param

class MyModel(FrogMlModel):

    def __init__(self):
        self._gamma = 'scale'
        log_param({"gamma": self._gamma})

    def build(self):
        # Load training data
        iris = datasets.load_iris()
        X, y = iris.data, iris.target
        X_train, X_test, y_train, y_test = train_test_split(X, y)

        # Model Training
        clf = svm.SVC(gamma=self._gamma)
        self.model = clf.fit(X_train, y_train)

    def predict(self, df):
        return self.model.predict(df)
```

## Using the CLI

You can add parameters to the build CLI command when you start a new build:

```
frogml models build \
    --model-id <model-id> \
    -P <key>=<value> -P <key>=<value> \
    <uri>
```

`<model-id>` - The model ID associated with the build.

`<key>` - The model parameter key.

`<value>` - The model parameter value.

`<uri>` - The frogml-based model URI.

## Logging Build Files

When executing a build, you can explicitly log files and attach them to a tag for reference (they can also be downloaded later). You can use this method to share files between models and builds.

<Callout icon="❗️" theme="error">
  **Important**

  `model_id` must be provided when logging parameters to files.
</Callout>

For example, you can persist the catboost classifier using pickle and add it to the logged files:

```python
import frogml.sdk.model.model_version_tracking
import pickle

def build():
    ...

    with open('model.pkl', 'wb') as handle:
        pickle.dump(self.catboost, handle, protocol=pickle.HIGHEST_PROTOCOL)

    frogml.log_file(from_path='model.pkl', tag='catboost_model')
```

Or, alternatively, import the `log_file` method directly:

```python
from frogml.core.model_loggers.artifact_logger import log_file

log_file(from_path='model.pkl', tag='catboost_model')
```

Now, in order to load that file, you need the file identifier, the tag, and the model and build where the file was persisted:

```python
from frogml.core.model_loggers.artifact_logger import load_file

load_file(to_path='model.pkl', tag='catboost_model', model_id='some_model_id', build_id='some_build_id')
```

The `to_path` parameter defines the location where you want to write the file inside the currently used Docker container.

Files can also be logged without a build context:

```python
from frogml.core.model_loggers.artifact_logger import log_file

log_file(from_path='model.pkl', tag='catboost_model', model_id='some_model_id')
```

<Callout icon="📘" theme="info">
  **Note**

  _**Size Limitation**_

  Currently, `log_file` allows for the logging of files to JFrog ML Cloud with a maximum size limit of 5GB and the `tag` should contain underscores `_`, not dashes `-`.
</Callout>

## Versioning Build Data

When you execute a build, you can store the build data:

```
import frogml

frogml.log_data(pd.dataframe, tag)
```

Or, alternatively, import the `log_data` method directly:

```
from frogml.core.model_loggers.data_logger import log_data

log_data(pd.dataframe, tag)
```

The data is exposed in the JFrog ML UI, and you can query the data and view the feature distribution - under the **Data** tab of a specific build.

For example:

```python
import frogml
from frogml import FrogMlModel
from sklearn import svm
from sklearn import datasets
from sklearn.metrics import f1_score
from sklearn.model_selection import train_test_split
import pandas as pd
from frogml.core.model_loggers.data_logger import log_data


class IrisClassifier(FrogMlModel):
    def __init__(self):
        self._gamma = 'scale'

    def build(self):
        # Load training data
        iris = datasets.load_iris()
        # Save training data
        log_data(iris, "training_data")
        X, y = iris.data, iris.target

        # Model Training
        clf = svm.SVC(gamma=self._gamma)
        self.model = clf.fit(X, y)

    def predict(self, df):
        return self.model.predict(df)
```

The data is saved under the build, and attached to the given tag.

<Callout icon="📘" theme="info">
  **Note**

  _**Supported Data Types**_

  The Dataframe supported types are the : `['object', 'uint8', 'int64', 'float64', 'datetime64', 'datetime64[ns]', 'datetime64[ns, UTC]', 'bool']`

  To modify a column's data type you can use the following syntax:

  `validation_df['column1'] = validation_df['column1'].astype('float64')`
</Callout>

## Versioning Data Outside Build

Similar to files, data can be logged without a specific build context. In order to do that, specify a `model_id` you'd like the `DataFrame` to be attached to:

```python
from frogml.core.model_loggers.data_logger import log_data
from pandas import DataFrame

df = DataFrame()
log_data(df, tag="some-tag", model_id="your-model-id", build_id="your-build-id")
```

## Loading Data from Builds

To access data logged during the build process, utilize the following JFrog ML function for downloading based on your model ID, build ID, and the specified data tag. This code can be executed either locally on your machine or in a remote workspace, making it optional to run within the context of a FrogML model build.

```python
from frogml.core.model_loggers.data_logger import load_data

df = load_data(tag="some-tag", model_id="your-model-id", build_id="your-build-id")
```

## Automatic Model Logging

During every build, the JFrog ML platform automatically logs the model as an artifact.

<Callout icon="⚠️" theme="warning">
  **Warning**

  **Objects must be pickled**

  Models logs do not works when your objects cannot be pickled.

  The automatic model logging works only when the class that extends `FrogMlModel` can be pickled using the `pickle.dump` function.

  If the model cannot be pickled, the build log will contain a warning message "Failed to log model." This error won't stop the build, and the trained model can be deployed in the JFrog ML platform.
</Callout>

You can retrieve the model using the `load_model` function. The function accepts two arguments: a model id and the build identifier. It returns an instance of the `FrogMlModel` class.

```python
from frogml.sdk.model_loggers.model_logger import load_model
from frogml.sdk.model.base import BaseModel

loaded_model: BaseModel = load_model('<your_model_id>', '<your_build_id>')
```

Remember that the current Python environment must contain all dependencies required to create a valid Python object from the pickled object. For example, if you logged a Tensorflow model, the Tensorflow library must be available (in the same version) when you call `load_model`.

## Automatic Dependency Logging

During every build, the JFrog ML platform runs `pip freeze` to log all of the dependencies used during the build.

<Callout icon="⚠️" theme="warning">
  **Warning**

  **Do not include your file `requirements.lock` in the `jfrogml_artifacts` directory.**

  **It will be overwritten!**
</Callout>
