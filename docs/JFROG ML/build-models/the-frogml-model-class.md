---
title: The FrogML Model Class
deprecated: false
hidden: false
metadata:
  robots: index
---
<br />

## The`FrogMlModel`

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

## Build

The `build` method defines the model training logic and is invoked once, on build time. In case the model should be trained on the FrogML platform - the function should include the training code invocation:

```
def build(self):
    iris = datasets.load_iris()
    X, y = iris.data, iris.target

    clf = svm.SVC(gamma=self._gamma)
    self._model = clf.fit(X, y)
```

## Predict

The `predict` method defines the serving logic, invoked on every prediction request:

```
@api()
def predict(self, df: pd.DataFrame) -> pd.DataFrame:
    return pd.DataFrame(data=self._model.predict(df), columns=['species'])
```

Notice that in this case we use pandas `DataFrame` for both the input and the output. For other options, see [Prediction Input & Output Adapters](/docs/prediction-input-output-adapters).

<Callout icon="📘" theme="info">
  **Note**

  **Inference Batching**

  By default, the endpoint doesn't batch predictions. You can control this configuration using the `MAX BATCH SIZE` parameter (default: 1).

  If you enable batching, your endpoint code must be ready to handle multiple model invocations during a single call to the `predict` method.
</Callout>

## `@api` Decorator

JFrogML's API decorator adds additional functionality to the `predict` method. There are currently 4 options:

| Parameter            | Type                | Description                                                                                                                                                                           | Default Value            |
| :------------------- | :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----------------------- |
| `analytics`          | `bool`              | Whether to activate JFrogML's built-in inference data collection mechanism, which streams all inference requests to the [FrogML Lake](/ai-ml/docs/inference-analytics).               | `True`                   |
| `feature_extraction` | `bool`              | Whether to activate the automatic feature extraction mechanism, pulling features from JFrog ML's feature store. For more info see  [Features in Inference](doc:feature-consumption) . | `False`                  |
| `Input Adapter`      | `BaseInputAdapter`  | To which format should the input request be serialized. For a list of supported adapters see [Input & Output Adapters](/docs/prediction-input-output-adapters)  .                     | `DataframeInputAdapter`  |
| `Output Adapter`     | `BaseOutputAdapter` | To which format should the output of the `predict` function be serialized. For a list of supported adapters see [Input & Output Adapters](/docs/prediction-input-output-adapters).    | `DataframeOutputAdapter` |

## Schema

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

## Initialize model

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

```python
def initialize_model(self):
    with open('model.pkl', 'rb') as infile:
        self._model = pickle.load(infile)

@api()
def predict(self, df: pd.DataFrame) -> pd.DataFrame:
    return pd.DataFrame(data=self._model.predict(df), columns=['species'])
```

## Accessing the FrogML Logger

To log statements during the build and deployment stages on the FrogML platform, you can utilize the FrogML `Logger` object. This is accessible through the utility method demonstrated below:

```python
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
