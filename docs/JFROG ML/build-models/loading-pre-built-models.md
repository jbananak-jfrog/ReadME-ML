---
title: Loading Pre-built Models
deprecated: false
hidden: false
metadata:
  title: Loading Pre-built Models
  description: >-
    The fastest way to start using FrogML is to deploy a model you have trained
    in the past as a FrogML service.
  legacyUUIDs:
    - UUID-54f36073-49e8-4202-1c51-067f639f0757
    - UUID-d26067a2-57d8-a583-a496-641ef0b53c24
  robots: index
---
The fastest way to start using FrogML is to deploy a model you have trained in the past as a FrogML service.

Let's assume that you have already trained the model and stored it in S3. The storage mechanism doesn't matter as long as you can download it using Python code.

## Creating a New Model

First, we have to create a new FrogML project and models:

```
frogml models create "Pre Trained Model" --project-key "examples"
```

Then we want to create an empty project template:

```
frogml models init .
```

## Adding Dependencies

In this example, we will use `conda` so we have to edit the `conda.yml` file and put the required libraries into the dependency list.

<Callout icon="📘" theme="info">
  **Note**

  FrogML uses `conda` for dependency management.

  Alternatively, you may use virtual environments such as pip `requirements.txt` files and `poetry` dependency manager.
</Callout>

For this pre-trained model example, we need pandas, scikit-learn, and catboost.

Additionally, we have to add boto3 because we will use it to download the pre-trained model.

conda.yml

```
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.9
  - pip
  - pandas
  - scikit-learn
  - catboost
  - boto3
```

## Loading Model Code

JFrog ML offers two ways of loading an existing model:

1. Use the `build()` function.
2. Use the `initialize_model()` function.

## Models in `build()`

The `build` function is more flexible. You can load not only the model but also run additional fine-tuning training. You can preprocess the training data for fine-tuning.

In general, you can do whatever you want. The only difference between full training and loading a pre-training model is loading a model from a file instead of creating the model in the code.

When the build method finishes running, all model class fields will be pickled. Those fields are loaded at the model serving stage and are available in the `predict` function.

## Models in `initialize_model()`

If you use the `initialize_model` function, we will load the model while starting the inference service.

The deployment will take longer if you load a model from an S3 file here.

On the other hand, using the `initialize_model` function lets you skip additional serialization/deserialization between the build and inference stages. If you already have the ready-to-use model in a pickle file, you can use the `initialize_model` function.

In the following sections, we will show you how to load the model from a file stored S3 in the `build` function and load a model from a pickle file included in the project directory.

## Loading Model Files

Let's see two ways for loading pre-trained models:

1. Download the model from S3.
2. Store the model as a pickle file in the build directory and load it from the file.

### Loading Models from S3

In the build function, instead of training the model, you can download and load the Python object from a file.

First, download the file.

Use the `boto3` client to get the file from S3. Note that, the FrogML secrets manager is used to pass the credentials. You can learn more about the secrets manager in the tutorial about credentials management.

```
import boto3
from frogml.core.clients.secret_service import  SecretServiceClient

def __download_the_model(self):

  secret_service = SecretServiceClient()
  aws_api_key = secret_service.get_secret('aws_api_key_secret_name')
  aws_secret_key = secret_service.get_secret('aws_secret_key_secret_name')
  aws_region = secret_service.get_secret('aws_region_secret_name')

  s3_client = boto3.client(
    's3',
    aws_access_key_id=aws_api_key,
    aws_secret_access_key=aws_secret_key,
    region_name=aws_region
  )
  s3_client.download_file('bucket_id', 'object_key', 'model_file.cbm')
```

After downloading the file, load it in memory and start using it. The model loading code depends on the library you use.

In the case of `catboost` it looks like this:

```
from frogml.sdk.model.base import BaseModel as FrogMlModel
from catboost import CatBoostClassifier

class TitanicSurvivalPrediction(FrogMlModel):
    def __init__(self):
        self.model = CatBoostClassifier()

    def build(self):
        self.__download_the_model()
        self.model.load_model('model_file.cbm')
```

Note: If we were using Tensorflow, we would have to download all model files to a new directory and load it like this:

```
model_path = 'the directory with the pb file and the variables'
self.model = keras.models.load_model(model_path)
```

Right now, our entire class should look like this:

```
from frogml.sdk.model.base import BaseModel as FrogMlModel
from frogml.core.clients.secret_service import  SecretServiceClient
from catboost import CatBoostClassifier
import boto3


class TitanicSurvivalPrediction(FrogMlModel):
    def __init__(self):
        self.model = CatBoostClassifier()

    def __download_the_model(self):
        secret_service = SecretServiceClient()
        aws_api_key = secret_service.get_secret('aws_api_key')
        aws_secret_key = secret_service.get_secret('aws_secret_key')
        aws_region = secret_service.get_secret('aws_region')

        s3_client = boto3.client(
            's3',
            aws_access_key_id=aws_api_key,
            aws_secret_access_key=aws_secret_key,
            region_name=aws_region
        )
        s3_client.download_file('bucket_id', 'object_key', 'model_file.cbm')

    def build(self):
        self.__download_the_model()
        self.model.load_model('model_file.cbm')
```

## Loading Models from Pickle

If you have your model in a pickle file, you can put it in the main directory and use the `initialize_model` method to load it.

In this case, the build method is not implemented, but it still needs to be included in the class.

We can have an empty implementation of the build method. Now, we can define the initialize_model method and load the model from a pickle file:

```python
import frogml
import pickle
from frogml.sdk.model.base import BaseModel as FrogMlModel
from catboost import CatBoostClassifier
import pandas as pd

class TitanicSurvivalPrediction(FrogMlModel):
    def __init__(self):
        self.model = CatBoostClassifier()

    def build(self):
        pass

    def initialize_model(self):
        with open('model.pkl', 'rb') as infile:
            self.model = pickle.load(infile)
```

## Adding Preprocessing

Every machine learning model running in production requires some preprocessing code that converts the data from the business domain into model-compatible values.

In FrogML models, that code is put in the predict function.

That is also the place where we call the model to obtain the predictions:

```python
@frogml.api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        df = df.drop(['PassengerId'], axis=1)
        return pd.DataFrame(self.model.predict_proba(df)[:, 1], columns=['Survived_Probability'])
```

## Building the Model

Now you have everything you need to deploy your pre-trained model as a FrogML model.

To start a build locally from your terminal, use the following command; this command will also automatically deploy your model after the build is complete.

```
frogml models build --model-id pre_trained_model . --deploy
```
