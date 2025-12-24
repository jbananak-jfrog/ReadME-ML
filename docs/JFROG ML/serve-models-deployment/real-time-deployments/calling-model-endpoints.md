---
title: Calling Model Endpoints
deprecated: false
hidden: false
metadata:
  robots: index
---
<br />

## Calling Model Endpoints

In this section, you'll learn how to effectively interact with and call real-time model endpoints using various SDKs and our REST API.

<Anchor label="JFrog ML Rest API" title="JFrog ML Rest API" href="/docs/jfrog-ml-rest-api">JFrog ML Rest API</Anchor>

<Anchor label="Python SDK" title="Python SDK" href="/docs/python-sdk">Python SDK</Anchor>

<Anchor label="Java SDK" title="Java SDK" href="/docs/java-sdk">Java SDK</Anchor>

<Anchor label="Go SDK" title="Go SDK" href="/docs/go-sdk">Go SDK</Anchor>

### JFrog ML Rest API

After deploying a FrogML-based model, you can use a REST client to request inferences from the model, which is hosted as a real-time endpoint.

#### Authentication Process

To access the REST client, you first need to generate an access token.

1. [Generate an access token](/governance/docs/access-tokens).
2. Set up your environment: Add the generated token to your environment by using the following command;

```
export TOKEN="<Auth Token>"
```

Make sure to replace `<Auth Token>` with the actual token you generated. After this, you will be able to use the REST client with your access token for authentication.

#### Inference Example

The following example demonstrates how to invoke the model `test_model`. This model accepts a feature vector containing three fields, and it returns a single output field called "score."

To illustrate this, we will use a `curl` command as a REST client.

Once a token is generated, invoke the model as follows:

```
export TOKEN=""

curl --location --request POST 'https://models.<environment_name>.qwak.ai/v1/test_model/predict' 
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer '$TOKEN'' \
    --header 'X-JFrog-Tenant-Id: <TENANT ID>' \
    --data '{"columns":["feature_a","feature_b","feature_c"],"index":[0],"data":[["feature_value",1,0.5]]}'
```

##### Inference for a Specific Variation

When working with variations, you can create an inference for a specific variation (endpoint) by appending the variation name to the URL as shown below:

```
curl --location --request POST 'https://models.<environment_name>.qwak.ai/v1/test_model/variation_name/predict' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer '$TOKEN'' \
    --header 'X-JFrog-Tenant-Id: <TENANT ID>'
    --data '{"columns":["feature_a","feature_b","feature_c"],"index":[0],"data":[["feature_value",1,0.5]]}'
```

### Python SDK

After deploying a real time model, your Python client applications can use this module to get inferences from the model hosted as a real-time endpoint.

#### Installation

The Python inference clients is a more lightweight part of `frogml-inference` package which contains only the modules that are required for inference. To install, run:

```
pip install frog        ml-inference
```

#### Inference Examples

The following example invokes the model `test_model`. The model accepts one feature vector which contains three fields and produces one output field named "score".

```
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

##### Testing Inference for a Specific Variation

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

##### Running Inference for a Different FrogML Environment

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

### Java SDK

After you deploy a FrogML-based model, your JVM-based client applications can use this module to get inferences from the model hosted as a real-time endpoint.

#### Inference Example

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

#### Installation

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

#### Gradle Configuration

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

#### Scala SBT Configuration

Please note, JFrog ML does not distribute `javadoc` or `sources` JAR files. To ensure seamless integration and prevent potential issues within your Scala IDE or sbt environment, it is recommended to proactively disable the automatic fetching or inclusion of these artifacts in your project settings.

```
resolvers += "Qwak Maven Repository" at "https://qwak.jfrog.io/artifactory/qwak-mvn"

libraryDependencies ++= Seq(
    .....
  "com.qwak.ai" % "qwak-inference-sdk" % "1.0-SNAPSHOT" classifier "",
  ws
)
```

#### Model metadata

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

### Go SDK

After you deploy a FrogML-based model, your Go-based client applications can use this module to get inferences from the model hosted as a real-time endpoint.

#### Installation

To install the SDK and its dependencies, run the following Go command:

```
go get github.com/qwak-ai/go-sdk/qwak
```

#### Inference examples

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
