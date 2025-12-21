---
title: Training Models
deprecated: false
hidden: false
metadata:
  title: Training Models
  description: Train your models on a scalable infrastructure using JFrog ML.
  legacyUUIDs:
    - UUID-041bed8a-0b67-0ae3-7b30-917d00d11743
    - UUID-1fd8463a-6a79-cdf0-14bf-a009f4949cfb
    - UUID-9b9c7a9e-541d-a8ba-9634-6c9b931dc762
    - UUID-bae5f600-5dca-5f88-bd12-bc81a323abcf
    - UUID-fae8901f-a98e-82b6-fdc6-e6d2596c6b3b
    - UUID-2242b8e9-d94f-a7d4-dcfd-ffc3b40ce5b7
    - UUID-89930889-c03e-6d6d-bcf4-d5f24fa9db1c
    - UUID-821dbd59-af20-7ea9-9a2e-2347361cc19f
  robots: index
---
Train your models on a scalable infrastructure using JFrog ML.

## Train Models on JFrog ML

JFrog ML let's you easily train models on a scalable infrastructure, either with CPU or GPU-based instances. Easily choose the resources you'd like to use and train your models with several easy commands. 🚀

Training takes place during the build phase on the model. Your training code should be placed inside the `build()` method, which is called only once during the build phase.

Training data can be ingested from the <Anchor label="Feature Store" target="_blank" href="https://jfrog.com/blog/what-is-a-feature-store-in-ml-and-do-i-need-one/">Feature Store</Anchor>, in an uploaded CSV file or directly from S3 or Parquet files.

In this section we will show you how to train your models using various examples.

## Train Models with GPUs

GPU instances provide high-performance resources to accelerate the model training process.

FrogML's GPU instances provide high-performance computing resources to accelerate the training process. 🚀

Easily customize your training resources to achieve faster training times and better results.

<Callout icon="⚠️" theme="warning">
  **Warning**

  Building your first model?

  Please refer to our**<Anchor label="Get Started with JFrog ML" title="Get Started with JFrog ML" href="/docs/get-started-with-jfrog-ml">Get Started with JFrog ML</Anchor>**guide if you're creating your first model. The guide provides step-by-step instructions on how to install all relevant dependencies to get you up and running.
</Callout>

### Training HuggingFace Models

Let's train a text classifier using a pre-trained HuggingFace model.

In this tutorial, we use a `distilbert` text classifier from HuggingFace and to train it using GPUs.

### Choosing the Correct GPU

Visit the <Anchor label="GPU Instance Sizes" title="Instance Sizes & ML Credits" href="/docs/instance-sizes---ml-credits">GPU Instance Sizes</Anchor> page to view the full specifications of FrogML's GPU instance selection.

### Project Dependencies

This is the content of our `conda.yml` file which contains the necessary dependencies for our GPU build.

conda.yml

```
channels:
  - defaults
  - conda-forge
  - huggingface
  - pytorch
dependencies:
  - python=3.9
  - pip
  - pandas=1.1.5
  - transformers
  - scikit-learn
  - datasets
  - pytorch
  - huggingface_hub
  - evaluate
```

### Adding Imports

We need to import all the relevant methods from FrogML and from the other packages we're using

```
import frogml
from frogml.sdk.model.base import BaseModel as FrogMlModel
import pandas as pd
import numpy as np
import evaluate
import torch
from datasets import load_dataset
from transformers import AutoTokenizer, AutoModelForSequenceClassification
from transformers import TrainingArguments, Trainer
```

### Initializing FrogML Model

The `FrogMlModel` is our base class that implements all relevant helper methods to build and deploy a model on FrogML.

In this example, we load the `distilbert-base-uncased` model from HuggingFace.

```
class HuggingFaceTokenizerModel(FrogMlModel):

    def __init__(self):
        model_id = "distilbert-base-uncased"
        self.tokenizer = AutoTokenizer.from_pretrained(model_id)
        self.model = AutoModelForSequenceClassification.from_pretrained(model_id, num_labels=2)
```

### Defining Model Build

The build method is called once and only during the model build phase. This method is called when generating a docker image of your model build.

```
def build(self):
        """
        The build() method is called once during the remote build process on JFrogML.
        We use it to train the model on the Yelp dataset
        """

        def tokenize(examples):
            return self.tokenizer(examples['text'],
                                  padding='max_length',
                                  truncation=True)

        dataset = load_dataset('yelp_polarity')

        print('Tokenizing dataset...')
        tokenized_dataset = dataset.map(tokenize, batched=True)

        print('Splitting data to training and evaluation sets')
        train_dataset = tokenized_dataset['train'].shuffle(seed=42).select(range(50))
        eval_dataset = tokenized_dataset['test'].shuffle(seed=42).select(range(50))

        # We don't need the tokenized dataset
        del tokenized_dataset
        del dataset

        # Defining parameters for the training process
        metric = evaluate.load('accuracy')

        # A helper method to evaluate the model during training
        def compute_metrics(eval_pred):
            logits, labels = eval_pred
            predictions = np.argmax(logits, axis=1)
            return metric.compute(predictions=predictions, references=labels)

        training_args = TrainingArguments(
            output_dir='training_output',
            eval_steps=1,  # Evaluate every step instead of every epoch
            num_train_epochs=1
        )

        # Defining all the training parameters for our tokenizer model
        trainer = Trainer(
            model=self.model,
            args=training_args,
            train_dataset=train_dataset,
            eval_dataset=eval_dataset,
            compute_metrics=compute_metrics
        )

        print('Training the model...')
        trainer.train()

        # Evaluate on the validation dataset
        eval_output = trainer.evaluate()

        # Extract the validation accuracy from the evaluation metrics
        eval_acc = eval_output['eval_accuracy']

        # Log metrics into JFrog ML
        frogml.log_metric({"val_accuracy" : eval_acc})
```

### Configuring Inference

The inference method is called when the model is invoked through the real-time endpoint, batch or streaming inference. This method is only triggered when the model is deployed or during local testing.

The inference method receives and returns a Pandas `DataFrame` by default. Provide it with <Anchor label="Input & Output Adapters" title="Prediction Input & Output Adapters" href="/docs/prediction-input---output-adapters">Input & Output Adapters</Anchor> to receive and return different data types.

```
    @frogml.api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        """
        The predict() method takes a pandas DataFrame (df) as input
        and returns a pandas DataFrame with the prediction output.
        """
        input_data = df['text'].to_list()

        # Get the device the model is on
        device = next(self.model.parameters()).device

        # Tokenize the input data using a pre-trained tokenizer
        tokenized = self.tokenizer(input_data,
                                   padding='max_length',
                                   truncation=True,
                                   return_tensors='pt')
        
        # Move tokenized inputs to the same device as the model
        tokenized = {k: v.to(device) for k, v in tokenized.items()}

        # Set model to evaluation mode
        self.model.eval()
        
        with torch.no_grad():
            response = self.model(**tokenized)

        # Convert logits to probabilities
        probabilities = response.logits.softmax(dim=1).cpu().numpy()
        
        # Return as a list of dictionaries
        result = []
        for prob in probabilities:
            result.append({
                'negative': float(prob[0]),
                'positive': float(prob[1])
            })
        
        return pd.DataFrame(result)
```

### Complete Model Code

The following code should be placed in the `model.py` and will allow you to build the HuggingFace based tokenizer we described in this tutorial.

model.py

```python
import frogml
from frogml.sdk.model.base import BaseModel as FrogMlModel
import pandas as pd
import numpy as np
import evaluate
import torch
from datasets import load_dataset
from transformers import AutoTokenizer, AutoModelForSequenceClassification
from transformers import TrainingArguments, Trainer

class HuggingFaceTokenizerModel(FrogMlModel):

    def __init__(self):
        model_id = "distilbert-base-uncased"
        self.tokenizer = AutoTokenizer.from_pretrained(model_id)
        self.model = AutoModelForSequenceClassification.from_pretrained(model_id, num_labels=2)

    def build(self):
        """
        The build() method is called once during the remote build process.
        We use it to train the model on the Yelp dataset
        """

        def tokenize(examples):
            return self.tokenizer(examples['text'],
                                  padding='max_length',
                                  truncation=True)

        dataset = load_dataset('yelp_polarity')

        print('Tokenizing dataset...')
        tokenized_dataset = dataset.map(tokenize, batched=True)

        print('Splitting data to training and evaluation sets')
        train_dataset = tokenized_dataset['train'].shuffle(seed=42).select(range(50))
        eval_dataset = tokenized_dataset['test'].shuffle(seed=42).select(range(50))

        # We don't need the tokenized dataset
        del tokenized_dataset
        del dataset

        # Defining parameters for the training process
        metric = evaluate.load('accuracy')

        # A helper method to evaluate the model during training
        def compute_metrics(eval_pred):
            logits, labels = eval_pred
            predictions = np.argmax(logits, axis=1)
            return metric.compute(predictions=predictions, references=labels)

        training_args = TrainingArguments(
            output_dir='training_output',
            eval_steps=1,  # Evaluate every step instead of every epoch
            num_train_epochs=1
        )

        # Defining all the training parameters for our tokenizer model
        trainer = Trainer(
            model=self.model,
            args=training_args,
            train_dataset=train_dataset,
            eval_dataset=eval_dataset,
            compute_metrics=compute_metrics
        )

        print('Training the model...')
        trainer.train()

        # Evaluate on the validation dataset
        eval_output = trainer.evaluate()

        # Extract the validation accuracy from the evaluation metrics
        eval_acc = eval_output['eval_accuracy']

        # Log metrics into JFrog ML
        frogml.log_metric({"val_accuracy" : eval_acc})

    @frogml.api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        """
        The predict() method takes a pandas DataFrame (df) as input
        and returns a pandas DataFrame with the prediction output.
        """
        input_data = df['text'].to_list()

        # Get the device the model is on
        device = next(self.model.parameters()).device

        # Tokenize the input data using a pre-trained tokenizer
        tokenized = self.tokenizer(input_data,
                                   padding='max_length',
                                   truncation=True,
                                   return_tensors='pt')
        
        # Move tokenized inputs to the same device as the model
        tokenized = {k: v.to(device) for k, v in tokenized.items()}

        # Set model to evaluation mode
        self.model.eval()
        
        with torch.no_grad():
            response = self.model(**tokenized)

        # Convert logits to probabilities
        probabilities = response.logits.softmax(dim=1).cpu().numpy()
        
        # Return as a list of dictionaries
        result = []
        for prob in probabilities:
            result.append({
                'negative': float(prob[0]),
                'positive': float(prob[1])
            })
        
        return pd.DataFrame(result)          
```

### Adding Integration Tests

We can define remote integration test on FrogML that are performed before saving the built model artifact in the model repository.

This code below should be copied into a new Python file under the tests folder in your local project: `tests/test_frogml_model.py`

test_frogml_model.py

```
import pandas as pd
from frogml.core.testing.fixtures import real_time_client

def test_realtime_api(real_time_client):
    feature_vector = [
        {
            'text': 'The best place ever!'
        }]

    classification = real_time_client.predict(feature_vector)
    
    # Verify the structure - should be a list of dictionaries
    assert isinstance(classification, list), f"Expected list, got {type(classification)}"
    assert len(classification) > 0, "Expected non-empty list"
    
    # Get the first prediction
    first_prediction = classification[0]
    assert isinstance(first_prediction, dict), f"Expected dict, got {type(first_prediction)}"
    assert 'positive' in first_prediction, f"Expected 'positive' key, got {list(first_prediction.keys())}"
    assert 'negative' in first_prediction, f"Expected 'negative' key, got {list(first_prediction.keys())}"
    
    # Check that the positive class probability is above 0.4
    positive_prob = first_prediction['positive']
    assert positive_prob > 0.4, f"Expected positive class probability > 0.4, got {positive_prob}"
    
    # Also verify that probabilities sum to approximately 1.0
    row_sum = first_prediction['negative'] + first_prediction['positive']
    assert abs(row_sum - 1.0) < 0.01, f"Expected probabilities to sum to 1.0, got {row_sum}"
```

### Initiating Remote GPU Build

It's now time to build the model!

Run the commands below in the terminal to build the model we created remotely.

#### Creating a Model on FrogML

```
frogml models create "Hugging Face Tokenizer Model" --project-key "examples"
```

### Building Your Models on GPUs

Our model is quite large, so we need to ask for a large GPU-based machine that has enough memory.

```
frogml models build --model-id hugging_face_tokenizer_model --instance "gpu.t4.xl" .
```

Visit the JFrog ML <Anchor label="GPU Instance Sizes" title="Instance Sizes & ML Credits" href="/docs/instance-sizes---ml-credits">GPU Instance Sizes</Anchor> page to choose the resources that fit your use case best. Each GPU type has its own configuration for pre-defined memory and number of CPUs.

<Callout icon="📘" theme="info">
  **Note**

  **Using GPU Spot Instances**

  JFrog ML uses EC2 Spot instances for GPU-based builds to keep costs low for users.

  As a result, it may take slightly longer for a GPU Spot Instance to become available.
</Callout>

### Building for GPU Deployments

When deploying a model on a GPU instance, we must verify that the model was build using a GPU compatible image. Build a model using a GPU compatible image installs additional dependencies and drivers.

Creating a GPU compatible image is simply done by adding the `--gpu-compatible` flag:

```
frogml models build --model-id <model-id> --gpu-compatible .
```

### Discovering GPU Cores

To see which GPUs were provided on your build machine, print the number of available GPUs:

```
# catboost
from catboost.utils import get_gpu_device_count
print(f'{get_gpu_device_count()} GPU devices')

# tensorflow
import tensorflow as tf
print(f'{len(tf.config.list_physical_devices("GPU"))} GPU devices')

# pytorch
import torch
print(f'{torch.cuda.device_count()} GPU devices')
```

Running the above command will build your model on a regular CPU instance, but will allow you to later deploy it on a GPU instance.

### Deploying GPU-trained Models on CPU

To facilitate the deployment of models trained on GPU environments onto CPU-based infrastructure, it is advised to adapt the model loading process within the `initialize_model()` method. Specifically, when employing `Torch` for model training, ensure the model is loaded to target the CPU explicitly:

my_model.py

```python
class MyModel(FrogMlModel):

  def init():
          ...
    
  def build():
          ...
    
  def initialize_model():
          self.model = torch.load("model.pkl", map_location=torch.device('cpu'))
```

Occasionally, you might encounter a Torch-related issue during model deserialization that disregards the specified CPU target, prompting a RuntimeError due to an attempt to deserialize on a CUDA device while CUDA is unavailable:

```
RuntimeError: Attempting to deserialize object on a CUDA device but torch.cuda.is_available() is False. If you are running on a CPU-only machine, please use torch.load with map_location=torch.device('cpu') to map your storages to the CPU.
```

By employing a custom _unpickler_ you can ensure the model is properly directed to the CPU during loading. The following example demonstrates how to implement such a solution:

my_model.py

```python
import pickle
import torch
import io

class CPU_Unpickler(pickle.Unpickler):
    def find_class(self, module, name):
        if module == 'torch.storage' and name == '_load_from_bytes':
                  # Redirects storage loading to CPU
            return lambda b: torch.load(io.BytesIO(b), map_location='cpu')
        else:
            # Default class resolution
            return super().find_class(module, name)


class MyModel(FrogMlModel):

  def init():
          ...
    
  def build():
          ...
    
  def initialize_model():
    #contents = pickle.load(f) becomes...
    with open("model.pkl", "rb") as handle: 
        self.model = CPU_Unpickler(handle).load()

        print (self.model.params)
```

This adjustment ensures the model, trained within a GPU-accelerated environment, is seamlessly transitioned for execution on CPU-based deployment targets.

## Using PyTorch with CUDA

When working with PyTorch on GPU instances, it's crucial to ensure that your library installation is compatible with the CUDA drivers installed on JFrog ML instances. This ensures optimal performance and compatibility with GPU resources.

Currently the JFrog ML GPU instances are provisioned with CUDA version 12.1 and below you will find instructions on using the latest versions of Torch compatible with the CUDA mentioned above.

### Installing Compatible PyTorch

To align PyTorch with the CUDA version on your instance, use the following index URL when adding the `pytorch` library to your dependencies configuration file, whether it's Conda, Pip (`requirements.txt`), or Poetry.

### In Workspaces

Use this command in your workspace environment:

```
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

### In Model Builds

For `requirements.txt`, your file should look like this:

requirements.txt

```
scipy
scikit-learn
pandas

--extra-index-url https://download.pytorch.org/whl/cu121
torch
torchvision
torchaudio
```

For Conda environments, here's an example configuration:

conda.yaml

```
name: your-conda-environment
channels:
  - defaults
  - conda-forge
  - huggingface
dependencies:
  - python=3.9
  - pip:
    - --extra-index-url https://download.pytorch.org/whl/cu121
    - torch
    - torchvision
    - torchaudio
  - transformers
  - accelerate
  - scikit-learn
  - pandas
```

Please note that the `conda.yaml` above is just an example, not all the dependencies are required.

### Verifying the Installation

After installation, confirm that `PyTorch` is utilizing the GPU. Add the following code snippet to your `FrogMlModel`. For training models, insert it at the start of the `build()` method. If loading a pre-trained model, place it in the `initialize_model()` method.

```
import torch

print("Torch version:",torch.__version__)

# Automatically use CUDA if available, else use CPU
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print(f"The PyTorch device used by the model is {device}\n")
```

This should output `cuda` as device in your FrogML model build logs, indicating that PyTorch is correctly set up to use the GPU.

### Troubleshooting

If you don't see `True` in your logs, check the **Code** tab within your Build page. Ensure that the dependency file is correctly recognised by the model and that the `requirements.lock` file reflects the appropriate versions for the Torch libraries.

## Building a Model with OpenCV

When you add the `opencv-python` library and import the `cv2` module, you might encounter the following error:

```
Exception: Error in importing module libGL.so.1: cannot open shared object file: No such file or directory
```

This issue occurs because the base Docker image does not include the necessary dependencies for OpenCV. To resolve this, you need to use a Docker image that supports OpenCV.

* For CPU instances: `public.ecr.aws/w8k8y6b6/qwak-base:0.0.29-cpu-opencv`
* For GPU instances: `public.ecr.aws/w8k8y6b6/qwak-base:0.0.14-gpu-opencv`

You can update the base image in one of two ways:

1. **Via Command Line**

   Add the `--base-image` parameter when building your model:

   ```
   frogml models build --base-image 'public.ecr.aws/w8k8y6b6/qwak-base:0.0.14-gpu-opencv'
   ```
2. **Via YAML Configuration**

   Update your YAML configuration file with the base image settings. Refer to our <Anchor label="Build Configurations" title="Build Configurations" href="/docs/build-configurations">Build Configurations</Anchor> page for more details.

   ```
   build_env:
     docker:
       base_image: public.ecr.aws/w8k8y6b6/qwak-base:0.0.14-gpu-opencv
   ```
