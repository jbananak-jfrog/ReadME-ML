---
title: Prediction Input & Output Adapters
deprecated: false
hidden: false
metadata:
  title: Prediction Input & Output Adapters
  description: Adapters help you customize input and output formats of your ML models.
  legacyUUIDs:
    - UUID-a562be07-2f8f-ae32-12d5-be5dbee4842a
    - UUID-fcb1409b-f141-035c-5562-252fc7a3cb1c
  robots: index
---
Adapters help customize the input and output formats of machine learning models. JFrog ML uses adapters to validate the formats of model inputs and outputs and to perform relevant type conversions.

This document provides a comprehensive list of the available input and output adapters.

<Callout icon="⚠️" theme="warning">
  **Warning**

  _**Adapters Support**_

  Input and output adapters are currently supported only in real-time and streaming deployed models, and **not in Batch-deployed models**. Efforts are ongoing to extend support for adapters across all deployment types.
</Callout>

## Adapter Types

### Image

In the model file, import the `ImageInputAdapter`.

```python
import frogml
import numpy as np
import pandas as pd
from frogml.sdk.model.adapters import ImageInputAdapter

@frogml.api(analytics=False, input_adapter=ImageInputAdapter())
def predict(self, input_data) -> pd.DataFrame:
```

The `predict` function receives a list of arrays containing the RGB properties of the image pixels. For example, passing a 28px x 28px image results in an array with shape (28, 28, 3). If a grayscale image is passed, the result will be a (28, 28, 1) array.

If the model is trained using grayscale images, but RGB values are passed in production,  it is necessary to convert the input to grayscale. For example:

```python
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

## File

Images can be passed as a file data stream and loaded (as a file) inside the `predict` function. This method works with any data format, not just images. The following example uses the same model as in the `ImageInputAdapter `example, but with a file adapter.

First you must add the `Pillow` library to the model dependencies and import the `Image` class and the input adapter:

```python
import numpy as np
import pandas as pd
from PIL import Image
from frogml.model.adapters import FileInputAdapter
```

Then, change the `input_adapter` parameter in the `frogml.api` decorator:

```python
@frogml.api(analytics=False, input_adapter=FileInputAdapter())
def predict(self, file_streams) -> pd.DataFrame:
```

In the `predict` function:

* iterate over the files in the `file_stream`,
* load them as images,
* convert them to grayscale, and
* resize them to the size required by the trained model.

After that, pass the image data to the model for predictions:

```python
result = []
for fs in file_streams:
    im = Image.open(fs).convert(mode="L").resize((28, 28))

    prediction_input = (np.expand_dims(im, 0))
    prediction = self.probability_model.predict(prediction_input)
    result.append(prediction[0])

return pd.DataFrame(result)
```

## String

To pass a single sentence to the ML model,  use the `StringInputAdapter`.

First, import the `StringInputAdapter`:

```python
from frogml.sdk.model.adapters import StringInputAdapter
```

Then, configure the `predict` function to use the input adapter:

```python
@frogml.api(analytics=False, input_adapter=StringInputAdapter())
def predict(self, texts) -> pd.DataFrame:
```

The `texts` variable contains a list of string values. Iterate over it and pass them to the model.

For example, if the `StringInputAdapter` was added to our example Pytorch text classifier:

```python
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

## JSON

For use in a front-end application, JSON can be sent to the server. Handle JSON automatically with the `JsonInputAdapter`.

First, import the adapter and configure the `predict` function:

```python
from frogml.model.adapters import JsonInputAdapter

@frogml.api(analytics=False, input_adapter=JsonInputAdapter())
    def predict(self, json_objects) -> pd.DataFrame:
```

Then, iterate over the `json_objects `and pass the text to the model:

```python
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

When sending a request to the deployed model, remember to specify  `Content-Type: application/json`.

## Proto

If you are using the protobuf library in your software, it may also be beneficial for communication with an ML model. It is common to use protobuf for both input and output formats and our example shows both.

Assuming a protobuf definition for input data:

```go
syntax = "proto3";

package frogml.demo;

option java_multiple_files = true;
option java_package = "com.qwak.ai.demo";

message ModelInput {

  int32 f1 = 1;

  int32 f2 = 2;
}
```

and output data:

```go
syntax = "proto3";

package frogml.demo;

option java_multiple_files = true;
option java_package = "com.frogml.ai.demo";

message ModelOutput {

  float prediction = 1;
}
```

Generate the protobuf classes for both the client application and the ML code. The ML code uses Python implementation. Store the Python files in the `input_pb` and the `output_pb` files in the `frogml_proto_demo` directory.

In the model class, import the protobuf class and the input adapter:

```go
from frogml.sdk.model.adapters import ProtoInputAdapter, ProtoOutputAdapter
from .frogml_proto_demo.input_pb import ModelInput
from .frogml.output_pb import ModelOutput
```

Next, configure the input and output adapters as decorators of the `predict` function:

```python
@frogml.api(
   analytics=False,
   input_adapter=ProtoInputAdapter(ModelInput),
   output_adapter=ProtoOutputAdapter(),
)
def predict(self, input) -> ModelOutput:
    ...
    return ModelOutput(prediction=prediction_from_the_model)
```

In the implementation, use the `ParseFromString` function to read a protobuf message, and remember to serialize  classes using the `SerializeToString` function.

```go
message = ModelInput(f1=1, f2=2).SerializeToString()
```

## TF Tensor

If the preprocessing code runs as a separate service, pass a Tensorflow tensor directly to the model using a `TfTensorInputAdapter`; import the adapter and configure the `predict` function's decorator:

```python
from frogml.sdk.model.adapters import TfTensorInputAdapter

@frogml.api(analytics=False, input_adapter=TfTensorInputAdapter())
def predict(self, tensor) -> pd.DataFrame:
```

To pass a tensor to a deployed model, send a JSON representation of the tensor. For example, using curl, the request would look like this:

```shell
curl -i –header "Content-Type: application/json" –request POST –data '{"instances": [1]}' jfrogml_rest_url
```

## Multi Input

The `MultiInputAdapter` supports automatic input format detection. To deploy a single model with **multiple different input adapters**, use a `MultiInputAdapter`to handle various input formats with a single model.

```python
from frogml.sdk.model.adapters import DefaultOutputAdapter, DataFrameInputAdapter, ImageInputAdapter, MultiInputAdapter

@frogml.api(
        analytics=False,
        input_adapter=MultiInputAdapter([ImageInputAdapter, DataFrameInputAdapter]),
        output_adapter=DefaultOutputAdapter(),
    )
```

(The alternative would be to create a copy of the model, change the input adapter, and deploy multiple copies.)

To use the `MultiInputAdapter` adapter, pass a list of adapters to its constructor. The `MultiInputAdapter` parses the data using the first compatible parser. In this example, if the input can be parsed as an image, ian image  will be received in the `predict` function. If not, a Pandas DataFrame will be received. If all parsers fail, the model returns an error.

Be cautious with the following adapter configuration:

```python
input_adapter=MultiInputAdapter([JsonInputAdapter, DataFrameInputAdapter]),
```

The `JsonInputAdapter` will successfully parse a JSON representation of a DataFrame!

## Numpy

A `NumpyInputAdapter` can automatically parse a JSON array as a Numpy array and reshape it to the desired structure. When configuring the `NumpyInputAdapter`, specify the content type and its shape:

```python
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

When the input adapter is configured as shown (in the above example), if the model receives the value `[[5,4,3,2]]`, the output will be equivalent to running `np.array([[5, 4, 3, 2]], dtype=np.int32).reshape(2, 2)`.

The `NumpyOutputAdapter` converts the returned output array directly to JSON without changing its structure. For example, if the model returns this Numpy array:

`np.array([[5, 4, 3, 2]], dtype=np.int32).reshape(2, 2)`,

it will be converted to: `[[5, 4], [3, 2]]`.

Starting from SDK version 0.9.87 it will return Numpy binary format .

## Default Output

The `DefaultOutputAdapter` enables returning multiple result formats from a single model. The adapter  automatically detects the type of the returned value.

```python
from frogml.sdk.model.adapters import DefaultOutputAdapter, ImageInputAdapter

@frogml.api(
    analytics=False,
    input_adapter=ImageInputAdapter(),
    output_adapter=DefaultOutputAdapter(),
)
def predict(self, input):
```

Note that the `DefaultOutputAdapter` doesn't work with Protobuf objects! To automatically detect the output type when returning DataFrames, JSONs, and Protobuf objects, the `AutodetectOutputAdapter` should be used.

## Json Output

When using JsonOutputAdapter to return Dict results, the output **must be iterable.**

```python
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

## Auto Detect Output

Automatic output format detection with Protobuf support operates like the `DefaultOutputAdapter`, but it can also handle Protobuf classes:

```python
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

## Data Frame Based Adapters

Pandas DataFrame is supported with the requested orientation.

```python
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

You can also just choose the data frame adapters with default values:

```python
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

In this case, the `DataFrameInputAdapter` attempt to automatically recognize the type of the input. For `DataFrameOutputAdapter` the output will be oriented by **records**.

## All Supported Adapters

The following is a list of all input and output adapters provided by JFrog ML:

| Output Adapters           | Input Adapters          |
| :------------------------ | :---------------------- |
| `DataFrameOutputAdapter`  | `DataFrameInputAdapter` |
| `DefaultOutputAdapter`    | `FileInputAdapter`      |
| `AutodetectOutputAdapter` | `ImageInputAdapter`     |
| `JsonOutputAdapter`       | `JsonInputAdapter`      |
| `ProtoOutputAdapter`      | `ProtoInputAdapter`     |
| `TfTensorOutputAdapter`   | `StringInputAdapter`    |
| `NumpyOutputAdapter`      | `TfTensorInputAdapter`  |
|                           | `NumpyInputAdapter`     |
|                           | `MultiInputAdapter`     |

<br />
