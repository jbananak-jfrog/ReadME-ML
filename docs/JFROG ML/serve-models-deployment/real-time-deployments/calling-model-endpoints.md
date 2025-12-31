---
title: Calling Model Endpoints
deprecated: false
hidden: false
metadata:
  robots: index
---
<br />

In this section, you'll learn how to effectively interact with and call real-time model endpoints using various SDKs and our REST API.

[JFrog ML Rest API](/docs/calling-model-endpoints#jfrog-ml-rest-api) | [Python SDK](/docs/calling-model-endpoints#python-sdk)| [Java SDK](/docs/calling-model-endpoints#java-sdk) | [Go SDK](/docs/calling-model-endpoints#go-sdk)

## JFrog ML Rest API

After deploying a FrogML-based model, you can use a REST client to request inferences from the model, which is hosted as a real-time endpoint.

### Authentication Process

To access the REST client, you first need to generate an access token.

1. [Generate an access token](/administration/docs/access-tokens).
2. Set up your environment: Add the generated token to your environment by using the following command;

```
export TOKEN="<Auth Token>"
```

Make sure to replace `<Auth Token>` with the actual token you generated. After this, you will be able to use the REST client with your access token for authentication.

### Inference Example

The following example demonstrates how to invoke the model `test_model`. This model accepts a feature vector with three fields and returns a single output field called "score".

Using`curl`as the REST client, invoke the model with your generated token:

```shell
export TOKEN=""

curl --location --request POST 'https://models.<environment_name>.qwak.ai/v1/test_model/predict' 
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer '$TOKEN'' \
    --header 'X-JFrog-Tenant-Id: <TENANT ID>' \
    --data '{"columns":["feature_a","feature_b","feature_c"],"index":[0],"data":[["feature_value",1,0.5]]}'
```

#### Inference for a Specific Variation

When working with variations, you can create an inference for a specific variation (endpoint) by appending the variation name to the URL as shown below:

```shell
curl --location --request POST 'https://models.<environment_name>.qwak.ai/v1/test_model/variation_name/predict' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer '$TOKEN'' \
    --header 'X-JFrog-Tenant-Id: <TENANT ID>'
    --data '{"columns":["feature_a","feature_b","feature_c"],"index":[0],"data":[["feature_value",1,0.5]]}'
```

## Python SDK

After deploying a real time model, your Python client applications can use this module to get inferences from the model hosted as a real-time endpoint.

### Installation

The Python inference client is a lightweight part of `frogml-inference` package, containing only the modules required for inference. To install, run:

```shell
pip install frogml-inference
```

### Inference Examples

The following example invokes the model `test_model`. The model accepts one feature vector which contains three fields and produces one output field named "score".

```python
from frogml_inference import RealTimeClient

model_id = "test_model"
feature_vector = [
   {
     "feature_a": "feature_value",
     "feature_b": 1,
     "feature_c": 0.5
   }]

client = RealTimeClient(model_id=model_id)
response = client.predict(feature_vector)
```

#### Testing Inference for a Specific Variation

You can optionally specify a variation name when working with the `RealtimeClient`.

```
from frogml_inference import RealTimeClient

model_id = "test_model"
feature_vector = [
   {
     "feature_a": "feature_value",
     "feature_b": 1,
     "feature_c": 0.5
   }]

client = RealTimeClient(model_id=model_id,
                        variation="variation_name")
response = client.predict(feature_vector)
```

#### Running Inference for a Different FrogML Environment

When working in a multi environment account, you need to specify a environment name when sending an inference to a non-default account using the `RealtimeClient`.

```
from frogml_inference import RealTimeClient
from frogml_inference.configuration import Session

Session().set_environment("staging")

model_id = "test_model"
feature_vector = [
   {
     "feature_a": "feature_value",
     "feature_b": 1,
     "feature_c": 0.5
   }]

client = RealTimeClient(model_id=model_id,
                        environment="staging")
response = client.predict(feature_vector)
```

## Java SDK

After you deploy a FrogML-based model, your JVM-based client applications can use this module to get inferences from the model hosted as a real-time endpoint.

### Installation

The Java Inference SDK is hosted on JFrog ML's internal maven repository.

#### Maven Configuration

To set up a Maven-based application that uses the Java Inference SDK, add the following sections to the projects `pom.xml`:

```
<project>

...

  <repositories>
    <repository>
      <id>qwak-mvn</id>
      <name>Qwak Maven Repository</name>
      <url>https://qwak.jfrog.io/artifactory/qwak-mvn</url>
    </repository>
  </repositories>

...

  <dependencies>
    <dependency>
      <groupId>com.qwak.ai</groupId>
      <artifactId>qwak-inference-sdk</artifactId>
      <version>1.0.16</version>
    </dependency>
  </dependencies>

</project>
```

### Inference Example

The following example invokes the model `test_model`. The model accepts one feature vector which contains three fields and produces one output field named "score".

```
RealtimeClient client = RealtimeClient.builder()
          .environment("env_name")
          .apiKey(API_KEY)
          .build();

PredictionResponse response = client.predict(PredictionRequest.builder()
          .modelId("test_model")
          .featureVector(FeatureVector.builder()
                .feature("feature_a", "feature_value")
                .feature("feature_b", 1)
                .feature("feature_c", 0.5)
                .build())
          .build());

Optional<PredictionResult> singlePrediction = response.getSinglePrediction();
double score = singlePrediction.get().getValueAsDouble("score");
```

### Gradle Configuration

To set up a Gradle-based application that uses the Java Inference SDK, add the following sections to the projects `build.gradle`:

```
repositories {
  maven {
    url "https://qwak.jfrog.io/artifactory/qwak-mvn"
  }
}

...

dependencies {
    ...
    implementation 'com.qwak.ai:qwak-inference-sdk:1.0-SNAPSHOT'
}
```

### Scala SBT Configuration

<Callout icon="📘" theme="info">
  The JFrog ML SDK does not distribute`javadoc` or `sources` JARs. You must configure your project to skip downloading these artifacts (proactively disabling automatic fetching) to prevent build errors.
</Callout>

```
resolvers += "Qwak Maven Repository" at "https://qwak.jfrog.io/artifactory/qwak-mvn"

libraryDependencies ++= Seq(
    .....
  "com.qwak.ai" % "qwak-inference-sdk" % "1.0-SNAPSHOT" classifier "",
  ws
)
```

### Model metadata

To retrieve the model metadata, use the `ModelMetadataClient`:

```
import com.qwak.ai.metadata.client.output.ModelMetadata;

...

ModelMetadataClient client = ModelMetadataClient.builder()
  .apiKey("YOUR QWAK API KEY")
  .build();
ModelMetadata metadata = client.getModelMetadata("MODEL NAME");
```

The `ModelMetadata` class has the following methods:

```
public Map<String, Object> getModel() # returns information about the model
public List<Map<String, Object>> getDeploymentDetails() # if the model is deployed, it returns data about deployment configuration
public Map<String, Map<String, Object>> getAudienceRoutesByEnvironment() # audience configuration per environment
public List<Map<String, Object>> getBuilds() # data about the DEPLOYED builds
```

## Go SDK

After you deploy a FrogML-based model, your Go-based client applications can use this module to get inferences from the model hosted as a real-time endpoint.

### Installation

To install the SDK and its dependencies, run the following Go command:

```
go get github.com/qwak-ai/go-sdk/qwak
```

### Inference examples

The following example invokes the model `test_model` which accepts one feature vector which contains three fields and produces one output field named "score".

```
package main

import (
    "fmt"
    "github.com/qwak-ai/go-sdk/qwak"
)


func main() {
    client, err := qwak.NewRealTimeClient(qwak.RealTimeClientConfig{
        ApiKey:      "api-key",
        Environment: "env-name",
    })

    if err != nil {
        fmt.Println("Errors occurred, Error: ", err)
    }

    predictionRequest := qwak.NewPredictionRequest("test_model").AddFeatureVector(
        qwak.NewFeatureVector().
            WithFeature("feature_a", "feature_value").
            WithFeature("feature_b", 1).
            WithFeature("feature_c", 0.5),
    )

    response, err := client.Predict(predictionRequest)
    if err != nil {
        fmt.Println("Errors occurred, Error: ", err)
    }

    val , err := response.GetSinglePrediction().GetValueAsInt("score")
    if err != nil {
        fmt.Println("Errors occurred, Error: ", err)
    }

    fmt.Println(val)
}
```
