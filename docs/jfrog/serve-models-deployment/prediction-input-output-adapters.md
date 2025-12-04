---
title: "Prediction Input & Output Adapters"
deprecated: false
hidden: false
metadata:
  title: "Prediction Input & Output Adapters"
  description: Adapters help you customize input and output formats of your ML models.
  robots: index
  legacyUUIDs:
    - UUID-a562be07-2f8f-ae32-12d5-be5dbee4842a
    - UUID-fcb1409b-f141-035c-5562-252fc7a3cb1c
---
Adapters help you customize input and output formats of your ML models.

JFrog ML uses adapters validate the model input and output formats and perform relevant type conversions.

This document lists to wide variety of available input and output adapters.

<Callout icon="⚠️" theme="warning">
**Warning**

***Adapters Support***

Please note that input and output adapters are currently supported only in real-time and streaming deployed models, and **not in Batch ones**. We're actively working to extend support for adapters across all deployment types.
</Callout>


### Adapter Types

#### Image

In the model file, we have to import the `ImageInputAdapter`.

```
import frogml
import numpy as np
import pandas as pd
from frogml.sdk.model.adapters import ImageInputAdapter

@frogml.api(analytics=False, input_adapter=ImageInputAdapter())
def predict(self, input_data) -> pd.DataFrame:
```

Now, the `predict` function, gets a list of arrays containing the RGB properties of the image pixels. If, for example, we pass a 28px x 28px image, we get an array with shape (28, 28, 3). Of course, if we pass a grayscale image, we will get a (28, 28, 1) array.

If you trained your model using grayscale pictures but pass RGB values in production, remember to convert the input to grayscale. For example like this:

```
@frogml.api(analytics=False, input_adapter=ImageInputAdapter())
def predict(self, input_data) -> pd.DataFrame:
    def rgb2gray(rgb):
        return np.dot(rgb[..., :3], [0.2989, 0.5870, 0.1140])

    result = []

    for image in input_data:
        gray = rgb2gray(image)
        gray = gray / 255.0

        prediction_input = (np.expand_dims(gray, 0))
        prediction = self.probability_model.predict(prediction_input)
        result.append(prediction[0])

    return pd.DataFrame(result)
```

### File

We can pass the image as a file data stream and load it as a file inside the `predict` function. Of course, sending a file works with any data format, not only images. In the following example, we will use the same model as in the ImageInputAdapter example, but with a file adapter.

Before we start, we have to add the `Pillow` library to the model dependencies and import the `Image` class and the input adapter

```
import numpy as np
import pandas as pd
from PIL import Image
from frogml.model.adapters import FileInputAdapter
```

Now, we can change the input\_adapter parameter in the `frogml.api` decorator:

```
@frogml.api(analytics=False, input_adapter=FileInputAdapter())
def predict(self, file_streams) -> pd.DataFrame:
```

In the `predict` function, we will:

* iterate over the files in the `file_stream`,
* load them as images,
* convert them to grayscale, and
* resize them to the size required by the trained model.

After that, we need to pass the image data to the model to get the prediction.

```
result = []
for fs in file_streams:
    im = Image.open(fs).convert(mode="L").resize((28, 28))

    prediction_input = (np.expand_dims(im, 0))
    prediction = self.probability_model.predict(prediction_input)
    result.append(prediction[0])

return pd.DataFrame(result)
```

### String

If we want to pass a single sentence to the ML model, we can use the `StringInputAdapter`.

First, import the StringInputAdapter.

```
from frogml.sdk.model.adapters import StringInputAdapter
```

Now, configure the predict function to use the input adapter:

```
@frogml.api(analytics=False, input_adapter=StringInputAdapter())
def predict(self, texts) -> pd.DataFrame:
```

The `texts` variable will contain a list of string values. We can iterate over it and pass them to the model.

For example, if we added the StringInputAdapter to our example Pytorch text classifier, it would look like this:

```
@frogml.api(analytics=False, input_adapter=StringInputAdapter())
def predict(self, texts) -> pd.DataFrame:
    text_pipeline = lambda x: self.vocab(self.tokenizer(x))

    responses = []
    for text in texts:
        with torch.no_grad():
            text = torch.tensor(text_pipeline(text))
            output = self.model(text, torch.tensor([0]))
            responses.append(output.argmax(1).item() + 1)

    return pd.DataFrame.from_dict({'label': responses, 'text': texts})
```

### JSON

If you want to use your model in a front-end application, you will probably send JSON to the server. You can handle the JSON automatically by using the `JsonInputAdapter`.

Import the adapter first and configure the `predict` function.

```
from frogml.model.adapters import JsonInputAdapter

@frogml.api(analytics=False, input_adapter=JsonInputAdapter())
    def predict(self, json_objects) -> pd.DataFrame:
```

Then, iterate over the json\_objects and pass the text to the model:

```
@frogml.api(analytics=False, input_adapter=JsonInputAdapter())
def predict(self, json_objects) -> pd.DataFrame:
    text_pipeline = lambda x: self.vocab(self.tokenizer(x))

    responses = []
    for json in json_objects:
        with torch.no_grad():
            text = torch.tensor(text_pipeline(json['text']))
            output = self.model(text, torch.tensor([0]))
            responses.append(output.argmax(1).item() + 1)

    return pd.DataFrame.from_dict({'label': responses, 'text': json_objects})
```

When sending a request to the deployed model, remember to specify the `Content-Type: application/json`.

### Proto

If you use the protobuf library in your software, you may also want to use it for communication with your ML model. It is common to use protobuf for both input and output formats, so our example will show both.

Let's assume that we have the following protobuf definition of the input data

```
syntax = "proto3";

package frogml.demo;

option java_multiple_files = true;
option java_package = "com.qwak.ai.demo";

message ModelInput {

  int32 f1 = 1;

  int32 f2 = 2;
}
```

and the output data:

```
syntax = "proto3";

package frogml.demo;

option java_multiple_files = true;
option java_package = "com.frogml.ai.demo";

message ModelOutput {

  float prediction = 1;
}
```

We have to generate the protobuf classes for both the client application and the ML code. Of course, the ML code uses Python implementation. We will store the Python files in the `input_pb` and the `output_pb` files in the `frogml_proto_demo` directory.

In the model class, we will have to import the protobuf class and the input adapter:

```
from frogml.sdk.model.adapters import ProtoInputAdapter, ProtoOutputAdapter
from .frogml_proto_demo.input_pb import ModelInput
from .frogml.output_pb import ModelOutput
```

Next, configure the input and output adapter as a decorator of the `predict` function:

```
@frogml.api(
   analytics=False,
   input_adapter=ProtoInputAdapter(ModelInput),
   output_adapter=ProtoOutputAdapter(),
)
def predict(self, input) -> ModelOutput:
    ...
    return ModelOutput(prediction=prediction_from_the_model)
```

In our implementation, we use the `ParseFromString` function to read a protobuf message, so remember to serialize your classes using the `SerializeToString` function.

```
message = ModelInput(f1=1, f2=2).SerializeToString()
```

### TF Tensor

If we have all of the preprocessing code running as a separate service, we can pass a Tensorflow tensor directly to the model using a `TfTensorInputAdapter`.

In this case, we import the adapter and configure the `predict` function's decorator:

```
from frogml.sdk.model.adapters import TfTensorInputAdapter

@frogml.api(analytics=False, input_adapter=TfTensorInputAdapter())
def predict(self, tensor) -> pd.DataFrame:
```

To pass a tensor to a deployed model, we must send a JSON representation of the tensor. For example, if we used curl, the request would look like this:

```
curl -i –header "Content-Type: application/json" –request POST –data '{"instances": [1]}' jfrogml_rest_url
```

### Multi Input

The `MultiInputAdapter` supports Automatic input format detection.

Sometimes we want to deploy a single model with multiple different input adapters. We could create a copy of the model, change the input adapter, and deploy multiple copies. However, we can also use a `MultiInputAdapter` to handle various input formats with a single model.

```
from frogml.sdk.model.adapters import DefaultOutputAdapter, DataFrameInputAdapter, ImageInputAdapter, MultiInputAdapter

@frogml.api(
        analytics=False,
        input_adapter=MultiInputAdapter([ImageInputAdapter, DataFrameInputAdapter]),
        output_adapter=DefaultOutputAdapter(),
    )
```

To use the `MultiInputAdapter` adapter, we must pass a list of adapters to its constructor. The `MultiInputAdapter` parses the data using the first compatible parser!

In our example, if a given input can be parsed as an Image, we will get an image in the predict function. If not, we will get a Pandas dataframe. If all parsers fail, the model returns an error.

Be careful with the following adapter configuration:

```
input_adapter=MultiInputAdapter([JsonInputAdapter, DataFrameInputAdapter]),
```

The `JsonInputAdapter` will successfully parse a JSON representation of a DataFrame!

### Numpy

A `NumpyInputAdapter` can automatically parse a JSON array as a Numpy array and reshape it to the desired structure. When we configure the `NumpyInputAdapter`, we have to specify the content type and its shape:

```
from frogml.sdk.model.adapters import NumpyInputAdapter, NumpyOutputAdapter

@frogml.api(
    analytics=False,
    input_adapter=NumpyInputAdapter(
        shape=(2, 2), enforce_shape=False, dtype="int32"
    ),
    output_adapter=NumpyOutputAdapter(),
    )
def predict(self, input):
```

If we configure the input adapter as in the example above, and send the following value to the model: `[[5,4,3,2]]`, we will get a result equivalent to running `np.array([[5, 4, 3, 2]], dtype=np.int32).reshape(2, 2)`.

The `NumpyOutputAdapter` converts the returned output array directly to JSON without changing its structure. For example, if the model returns this Numpy array: `np.array([[5, 4, 3, 2]], dtype=np.int32).reshape(2, 2)`, it will get converted to: `[[5, 4], [3, 2]]`.

Starting from Sdk version 0.9.87 it will return numpy binary format .

### Default Output

With `DefaultOutputAdapter` we can return multiple result formats from a single model. The adapter will automatically detect the type of the returned value.

```
from frogml.sdk.model.adapters import DefaultOutputAdapter, ImageInputAdapter

@frogml.api(
    analytics=False,
    input_adapter=ImageInputAdapter(),
    output_adapter=DefaultOutputAdapter(),
)
def predict(self, input):
```

Note that the DefaultOutputAdapter doesn't work with Protobuf objects! To automatically detect the output type when your code returns DataFrames, JSONs, and Protobuf objects, you need to use the `AutodetectOutputAdapter`.

### Json Output

With `JsonOutputAdapter` we can return `Dict` results, but **the result has to be iterable**

```
from frogml.sdk.model.adapters import ProtoInputAdapter, AutodetectOutputAdapter

@frogml.api(
        analytics=False,
        input_adapter=ProtoInputAdapter(ModelInput),
        output_adapter=AutodetectOutputAdapter(),
    )
    def predict(self, df) -> JsonOutputAdapter:
        ...
        return [{"result": ...}]
```

### Auto Detect Output

Automatic output format detection with Protobuf support

This adapter works like the `DefaultOutputAdapter`, but it can also handle Protobuf classes:

```
from frogml.sdk.model.adapters import ProtoInputAdapter, AutodetectOutputAdapter

@frogml.api(
        analytics=False,
        input_adapter=ProtoInputAdapter(ModelInput),
        output_adapter=AutodetectOutputAdapter(),
    )
    def predict(self, df) -> ModelOutput:
        ...
        return [ModelOutput(prediction=result)]
```

### Data Frame Based Adapters

Pandas Dataframe is supported with requested orient.

```
import pandas as pd
from frogml.sdk.model.adapters import DataFrameInputAdapter, DataFrameOutputAdapter

  @frogml.api(
      analytics=False,
      input_adapter=DataFrameInputAdapter(input_orient="split"),
      output_adapter=DataFrameOutputAdapter(output_orient="records"),
  )
  def predict(self, df) -> pd.DataFrame:
      # ... your prediction logic here ...

      # Constructing a DataFrame with the inference results
      predictions_df = pd.DataFrame({'prediction': result})

      return predictions_df
```

Note that the you can just choose the data frame adapters with default value like below:

```
from frogml.sdk.model.adapters import DataFrameInputAdapter, DataFrameOutputAdapter

    @frogml.api(
        analytics=False,
        input_adapter=DataFrameInputAdapter(),
        output_adapter=DataFrameOutputAdapter(),
    )
  def predict(self, df) -> pd.DataFrame:
      # ... your prediction logic here ...

      # Constructing a DataFrame with the inference results
      predictions_df = pd.DataFrame({'prediction': result})

      return predictions_df
```

In this case, the `DataFrameInputAdapter` will try to automatically recognize the type of the input. For `DataFrameOutputAdapter` the output will be oriented by **records**.

### All Supported Adapters

The following is a list of all input and output adapters provided by JFrog ML:

### Output Adapters

* `DataFrameOutputAdapter`
* `DefaultOutputAdapter`
* `AutodetectOutputAdapter`
* `JsonOutputAdapter`
* `ProtoOutputAdapter`
* `TfTensorOutputAdapter`
* `NumpyOutputAdapter`

### Input Adapters

* `DataFrameInputAdapter`
* `FileInputAdapter`
* `ImageInputAdapter`
* `JsonInputAdapter`
* `ProtoInputAdapter`
* `StringInputAdapter`
* `TfTensorInputAdapter`
* `NumpyInputAdapter`
* `MultiInputAdapter`
