---
title: Real-Time Deployments
deprecated: false
hidden: false
metadata:
  title: Real-Time Deployments
  description: >-
    JFrog ML real time models deploy your ML models with a lightweight, simple
    and scalable REST API wrapper.
  legacyUUIDs:
    - UUID-3c63c18f-5310-352e-82a2-2d3d5745f121
    - UUID-e5bdcc6e-6e47-c694-4572-8cf8839a9f97
    - UUID-4e73c999-bd0f-d756-06f3-0c4514774d77
    - UUID-338bfea3-f623-f1fb-39b7-7a83c0f19e11
    - UUID-dd1334a6-27b3-ba6a-76c9-c3b78de3977e
    - UUID-d9751b8e-1a73-3551-d7e1-3bb6780fcd91
    - UUID-4e421dfa-c842-f103-5df6-5bdc43943d32
    - UUID-55dfd710-8227-8992-1a28-9e88b4a5dcfb
    - UUID-fc130131-9381-e3ad-0936-4291d7036971
    - UUID-197a0e24-0460-a3e9-92ea-dfa947ebb4fe
    - UUID-84498bdf-d56a-182c-3bab-73108d55ec40
    - UUID-5477e288-db59-3da4-1926-144155bfbf0e
    - UUID-b46d4927-5000-bfe1-810c-28ee22695397
    - UUID-b8554316-7a57-70e5-95c0-3684250ca875
    - UUID-d550d373-13f6-b540-2347-abaeaaa9b19f
    - UUID-356ba223-34d5-7de8-0e34-3fff4abc9ef3
    - UUID-1f5286c5-1b85-e56e-1658-4fd1c4cffbb8
    - UUID-39d33214-4ef6-635c-0589-2999c29e493d
    - UUID-46bb01ad-e31c-ed1d-9f1f-60d1bef978bc
    - UUID-66ff6dc6-8e6e-0638-52b5-83c9fb2e284c
    - UUID-90ee98ef-d78b-b04f-d588-aad4e8e5cd26
    - UUID-8f76ae49-10fe-19b7-e687-e6e2ed55d867
    - UUID-a4778413-c6ba-0a65-76bc-7e2ea3e232b7
    - UUID-9349a4d4-0bc3-9449-dce0-f3c4f573b618
  robots: index
---
JFrog ML real time models deploy your ML models with a lightweight, simple and scalable REST API wrapper.

We set up the network requirements and deploy your model on a managed Kubernetes cluster, allowing you to leverage auto-scaling and security. JFrog ML also adds a suite of monitoring tools, simplifying the process of managing your model performance.

<Image alt="Real-time model deployment overview" border={false} src="https://files.readme.io/c22e14438f25fe2ee4e3b4a00a29a8e5206e9356f795e0290e3d6251393c2c4e-uuid-5a77810a-0d93-ea20-6692-73b7b87032e3.png" />

## Deploying Real-time Models from the UI

<Image alt="Deploying a real-time model from the UI" border={false} src="https://files.readme.io/c37504c48ecaf10085959181c6692b60a42ebe123e004704f8af89c3c79dff23-uuid-ba434fe8-6697-f02d-b1e0-c884dee29695.gif" />

▶ **To deploy a real-time model from the UI:**

1. Select **Models** in the left navigation bar
2. Select a project and a model.
3. Select the **Builds** tab.
4. Choose a build and click **Deploy**.
5. Select **Realtime** and then click **Next**.
6. Configure your real-time deployment by selecting the instance type and the initial number of replicas.
7. The **Advanced settings** configuration include additional options such as environment variables, invocation timeouts, and more.

## Deploying Real-time Models from the CLI

To deploy a model in real-time mode from the CLI, populate the following command template:

```
frogml models deploy realtime \
    --model-id <model-id> \
    --build-id <build-id> \
    --pods <pods-count> \
    --instance <instance-type> \
    --timeout <timeout-ms> \
    --server-workers <workers> \
    --variation-name <variation-name> \
    --daemon-mode <bool>
```

For example, for the model built in the <Anchor label="Get Started with JFrog ML" title="Get Started with JFrog ML" href="/docs/get-started-with-jfrog-ml">Get Started with JFrog ML</Anchor> section, the deployment command is:

```
frogml models deploy realtime \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --pods 2 \
    --instance small \
    --timeout 3000 \
    --server-workers 4 \
    --variation-name default \
    --daemon-mode false
```

<Callout icon="📘" theme="info">
  **Note**

  **Note:** The deployment command is executed asynchronously by default and does not wait for the deployment to complete. To execute the command synchronously use the `--sync` flag.
</Callout>

## Deploying a Real-time Model Using GPUs

Realtime models can be deployed on GPU instances, simply by selecting a [GPU Instance](/docs/instance-sizes-ml-credits#gpu-instances) from the available options.

```
frogl models deploy realtime \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --pods 4 \
    --instance gpu.a10.xl \
    --timeout 3000 \
    --server-workers 4 \
    --variation-name default \
    --daemon-mode false
```

## Configuring Real-time Models

The following table contains the possible parameters and variables for deploying real-time models.

| Parameter                       | Description                                                                                                                                                                                                                                                                                                                                                  | Default  |
| :------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- |
| Model ID [**Required**]         | The Model ID as displayed on the model header.                                                                                                                                                                                                                                                                                                               |          |
| Build ID [**Required**]         | The JFrog ML-assigned build ID.                                                                                                                                                                                                                                                                                                                              |          |
| Variation name                  | The name of the variation to deploy the build on.                                                                                                                                                                                                                                                                                                            | default  |
| Initial number of replicas      | The number of <Anchor label="k8s pods" target="_blank" href="https://kubernetes.io/docs/concepts/workloads/pods/">k8s pods</Anchor> to be used by the deployment. Each pod contains an HTTPS server, where a load balancer splits the traffic between them.                                                                                                  | 1        |
| Instance                        | The required instance to deploy the model, either a CPU based instance or a GPU based instance.                                                                                                                                                                                                                                                              | Small    |
| Timeout                         | The number of milliseconds required for an API server request to time out.                                                                                                                                                                                                                                                                                   | 1000(ms) |
| Concurrent workers              | The number of <Anchor label="Gunicorn workers" target="_blank" href="https://docs.gunicorn.org/en/stable/settings.html#worker-processes">Gunicorn workers</Anchor> handling requests. A positive integer is generally in the 2-4 x $(NUM_CORES) range. You may want to vary this a bit to find the optimal value for your particular application’s workload. | 2        |
| Daemon mode                     | Whether or not to <Anchor label="Daemonize" target="_blank" href="https://docs.gunicorn.org/en/stable/settings.html#daemon">Daemonize</Anchor> the Gunicorn process. Detaches the server from the controlling terminal and enters the background.                                                                                                            | Enabled  |
| IAM role ARN                    | The user-provided AWS custom IAM role.                                                                                                                                                                                                                                                                                                                       |          |
| Max batch size                  | The maximal allowed batch size.                                                                                                                                                                                                                                                                                                                              | 1        |
| Timeout                         | The prediction request timeout.                                                                                                                                                                                                                                                                                                                              | 5000(ms) |
| Service Account Key Secret Name | The service account key secret name to connect with Google cloud provider.                                                                                                                                                                                                                                                                                   | None     |
| Purchase option                 | Rather to use spot/ondemand.                                                                                                                                                                                                                                                                                                                                 | spot     |

<Callout icon="⚠️" theme="warning">
  **Warning**

  _**Worker Memory Allocation**_

  When deploying workers through an HTTP web server, it's essential to understand that each worker operates in its isolated memory space. Consequently, every worker independently loads a model instance into memory. This characteristic should be carefully considered when determining the required memory capacity for your chosen instance type, ensuring sufficient resources are available for all worker models to load and function optimally.
</Callout>

## Using Custom AWS IAM Role

In some cases, a model needs to access external services during the runtime. If your model requires access to AWS resources, a custom AWS IAM role can be passed during the deployment process.

The IAM role should be created with the following trust policy:

```python
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "ArnLike": {
          "aws:PrincipalArn": "arn:aws:iam::<account-id>:role/qwak-eks-base*"
        }
      }
    }
  ]
}
```

The IAM role ARN can be passed directly to a deployment using the `--iam-role-arn` flag. For example:

```python
frogml models deploy realtime \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --pods 4 \
    --instance small \
    --timeout 3000 \
    --server-workers 4 \
    --variation-name default \
    --daemon-mode false \
    --iam-role-arn arn:aws:iam::<account-id>:role/<role-name>
```

## Deploying Real-time Models Locally

To run the deployment locally using a local Docker engine, use the `--local` flag. For example:

```
frogml models deploy realtime \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --local
```

<Callout icon="📘" theme="info">
  **Note**

  **Note:** Deploying models locally is only available for locally generated builds using the `--no-remote` flag.
</Callout>

## Real-time Model Inference

Once you have successfully deployed a real-time model, you can use the JFrog ML Inference SDK to perform invocations.

In this example, we'll invoke a model via the <Anchor label="Python SDK" title="Python SDK" href="/docs/python-sdk">Python SDK</Anchor>, which can be easily installed using:

```
pip install frogml-inference
```

Model inference parameters are model specific.

For the below model, assuming it was built and deployed successfully as a real-time endpoint, with the model ID `iris_classifier`:

```python
from frogml import api, FrogMlModel
from sklearn import svm, datasets
import pandas as pd

class IrisClassifier(FrogMlModel):

    def __init__(self):
        self._gamma = 'scale'
        self._model = None

    def build(self):
        # load training data
        iris = datasets.load_iris()
        X, y = iris.data, iris.target

        # Model Training
        clf = svm.SVC(gamma=self._gamma)
        self._model = clf.fit(X, y)

    @api()
    def predict(self, df: pd.DataFrame) -> pd.DataFrame:
        return pd.DataFrame(data=self._model.predict(df), columns=['species'])
```

A prediction call from the JFrog ML Python SDK is:

```python
from frogml_inference import RealTimeClient

model_id = "iris_classifier"
feature_vector = [
   {
      "sepal_width": 3,
      "sepal_length": 3.5,
      "petal_width": 4,
      "petal_length": 5
   }]

client = RealTimeClient(model_id=model_id)
response = client.predict(feature_vector)
```

## Monitoring Real-time Endpoints

JFrog ML endpoints are deployed on Kubernetes, coupled with advanced monitoring tools for production-grade readiness.

JFrog ML comes bundled with Grafana and Prometheus to provide monitoring dashboards, and ElasticSearch for log collection, amongst other tools.

The following health metrics appear in the model **Overview** tab:

<Image alt="Model health monitoring dashboard" border={false} src="https://files.readme.io/8de63ece4bf79ce77ad93a33db42bde236adfd2ca428c31b2264933be23e0a8f-uuid-3c1b994a-ff06-7bdc-d129-5bfce1b81001.png" />

In addition, you can follow and search the applicable logs produced by your model in the **Logs** tab:

<Image alt="Model logs tab" border={false} src="https://files.readme.io/e3e6dc87c0d361da0419c49706cd4fc07c9d483534a7ddd2990f755ee8216a38-uuid-96057c91-dc54-213f-69da-34593108ce29.png" />

## Auto-scaling Real-time Models

To attach a new auto scaling to a running model:

### 1. Create a config file:

```python
api_version: v1
spec:
  model_id: <model-id>
  variation_name: <variation-name>
  auto_scaling:
    min_replica_count: 1
    max_replica_count: 10
    polling_interval: 30
    cool_down_period: 300
    triggers:
      prometheus_trigger:
        - query_spec:
            metric_type: <cpu/gpu/memory/latency/error_rate/throughput>
            aggregation_type: <min/max/avg/sum>
            time_period: 30
          threshold: 60
```

### 2. Run the following command:

```
frogml models autoscaling attach -f config.yaml
```

## Configuration

| Parameter                                  | Description                                                                                                                                                                                      | Default Value                                |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------- |
| min_replica_count (integer)                | The minimum number of replicas will scale the resource down to                                                                                                                                   |                                              |
| max_replica_count (integer)                | The maximum number of replicas of the target resource                                                                                                                                            |                                              |
| polling_interval (integer)                 | This is the interval to check each trigger on                                                                                                                                                    | 30 sec                                       |
| cool_down_period (integer)                 | The period to wait after the last trigger reported active before scaling the resource back to 0                                                                                                  | 300 sec                                      |
| metric_type (prometheus_trigger)           | The type of the metric                                                                                                                                                                           | cpu/gpu/memory/latency/error_rate/throughput |
| aggregation_type (prometheus_trigger)      | The type of the aggregation                                                                                                                                                                      | min/max/avg/sum                              |
| time_period (integer) (prometheus_trigger) | The period to run the query - value in minutes                                                                                                                                                   |                                              |
| threshold (integer) (prometheus_trigger)   | Value to start scaling for. cpu - usage in percentages, gpu - usage in percentages, memory - value in bytes, Latency - value in ms, Error Rate - usage in percentages, Throughput - usage in RPM |                                              |

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

## Deployment Strategies

ML models deployed as a real-time service support an advanced traffic management tool called variations. The tool allows you to perform a canary release of a new model or to shadow deploy a model.

A variation is an additional identifier through which you can manage the amount of traffic routed to a specific deployed build. You can deploy multiple builds per model by assigning builds to different variations.

Shadow deployment lets you test a model using production data without returning the predictions produced by the model to the caller. The JFrog ML platform still logs all requests and model responses, so you can review them and evaluate the model performance.

#### Deploying a Real-Time Model with Variations

When you deploy your first model, by default the variation name will be "default".

The first build automatically receives 100% of the traffic.

#### Deploying an Additional Variation

To deploy an additional variation first you must <Anchor label="create an audience" title="Traffic Splitting with Audiences and Variations" href="/docs/traffic-splitting-with-audiences-and-variations">create an audience</Anchor>.

Then you will be able to attach it to the model in the deployment process.

When deploying an additional variation you are prompted to enter a variation name. Choose a name that adheres to the following rules:

* Contains no more than 36 characters.
* Contains only lowercase alphanumeric characters, dashes or periods.
* Starts with an alphanumeric character.
* Ends with an alphanumeric character.

To deploy an additional variation:

1. Click the deploy button next to the build you want to deploy.
2. In the deployment popup, enter a new variation name and click **create new variation** in the select.
3. In the tab **Traffic Control** attach the new or the existing variations to the audience.
4. Ensure that the total traffic percentages add up to 100% in each audience.
5. Select a fallback variation. (can be either the new one or the existing one).
6. Click **Deploy**.

#### Replacing an Existing Variation

To replace an existing variation:

1. In the deployment popup, choose the variation you want to replace.
2. Modify the deployed model. You can replace any variation, including the default variation. The next tab of **Traffic Control** also allows you to modify the percentage of traffic.

#### Deployment via the CLI

To deploy a model with variations via the CLI, use this command:

```
frogml models deploy realtime --from-file <config-file-path>
```

The configuration file should look like this:

```
model_id: <model-id>
build_id: <build-id-to-deploy>
realtime:
  variation_name: <The variation name being deployed>
  audiences:
    - id: <The audience id>
      name: <The audience name>
      routes:
        - variation_name: <First variation>
          weight: 20
          shadow: false
        - variation_name: <The variation name being deployed>
          weight: 80
          shadow: false
  fallback_variation: <One of the variations>
```

* If you use `--variation-name` in the CLI command, you don't have to pass the `variation_name` in the configuration file.
* When you deploy your first build, you don't have to pass any variation-related data. You can also pass just the variation name parameter. Traffic is automatically adjusted to 100%.
* If you are using the configuration file, you must pass all the existing variations, regardless of whether you modify them or not.

#### Inference on a Specific Variation

You can run an inference against a specific variation by using one of the clients:

#### Python Runtime SDK

```
import pandas as pd
from frogml_inference import RealTimeClient

model_id = "test_model"
feature_vector = [
   {
     "feature_a": "feature_value",
     "feature_b": 1,
     "feature_c": 0.5
   }]

client = RealTimeClient(model_id=model_id, variation="variation_name")
response: pd.DataFrame = client.predict(feature_vector)
```

#### Java Inference SDK

```
RealtimeClient client = RealtimeClient.builder()
          .environment("env_name")
          .apiKey(API_KEY)
          .build();

PredictionResponse response = client.predict(PredictionRequest.builder()
          .modelId("test_model")
          .variation("variation_name")
          .featureVector(FeatureVector.builder()
                .feature("feature_a", "feature_value")
                .feature("feature_b", 1)
                .feature("feature_c", 0.5)
                .build())
          .build());

Optional<PredictionResult> singlePrediction = response.getSinglePrediction();
double score = singlePrediction.get().getValueAsDouble("score");
```

#### REST API

```
curl --location --request POST 'https://models.<environment_name>.qwak.ai/v1/test_model/<variation_name>/predict' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <Auth Token>' \
     --data '{"columns":["feature_a","feature_b","feature_c"],"index":[0],"data":[["feautre_value",1,0.5]]}'
```

#### Targeting A Specific Audience

###### PythonClient

You can run an inference against a specific audience by using the python sdk:

```
client.predict(feature_vector, metadata: {<key>:<value>})
```

You need to pass a key-value dictionary, its value will lead the traffic to the wanted audiences by its conditions.

###### REST API

```
curl --location --request POST 'https://models.<environment_name>.qwak.ai/v1/test_model/<variation_name>/predict' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <Auth Token>' \
     --header "<key>: <value>" \
     --data '{"columns":["feature_a","feature_b","feature_c"],"index":[0],"data":[["feature_value",1,0.5]]}'
```

### Traffic Splitting with Audiences and Variations

JFrog ML provides multiple ways to split traffic and run A/B tests using Audiences and Variations. Using JFrog ML, you can segment traffic, compare different variations, and make data-driven decisions to enhance the performance of your models.

#### Segmenting Traffic with Audiences

Audiences are a powerful tool for categorizing traffic on predefined client-side metadata conditions. Configured at the JFrog ML Account level, they apply to all real-time models within the Account.

By default, requests that do not meet any specific Audience criteria are routed to the `fallback` Audience.

<Callout icon="❗️" theme="error">
  **Important**

  Audience details must be included in the metadata or header fields at each inference request.
</Callout>

##### Creating an Audience

Audiences are created using YAML files and managed via the FrogML CLI.

Below is an example of an Audience that categorizes users from New York aged between 10 and 30

The first condition matches the `location` field to be `new-york`. This match has to be exact, it's not a regex.

The second condition is binary, where the `age` key is in the range specified with `first_operand` and `second_operand`.

```
api_version: v1
spec:
  audiences:
    - name: New-York
      description: Users from New York aged 10-30
      conditions:
        unary:
          - key: location
            operator: UNARY_OPERATOR_TYPE_EXACT_MATCH
            operand: new-york
        binary:
          - key: age
            operator: BINARY_OPERATOR_TYPE_RANGE_MATCH
            first_operand: 10
            second_operand: 30
```

Each audience definition must include:

* **Name**: A display name of the audience used on the JFrog ML platform UI.
* **Description**: A general description of the audience.
* **Conditions**: A list of conditions with an AND operand between them.

##### Defining Conditions

The following condition types are available when creating an audience:

###### Unary Operators

1. `UNARY_OPERATOR_TYPE_EXACT_MATCH`: Matches an exact value.
2. `UNARY_OPERATOR_TYPE_SAFE_REGEX_MATCH`: Matches using a regular expression.
3. `UNARY_OPERATOR_TYPE_PRESENT_MATCH`: Checks for the presence of a key.
4. `UNARY_OPERATOR_TYPE_PREFIX_MATCH`: Matches values that start with a specified prefix.
5. `UNARY_OPERATOR_TYPE_SUFFIX_MATCH`: Matches values that end with a specified suffix.
6. `UNARY_OPERATOR_TYPE_CONTAINS_MATCH`: Matches values containing a specified substring.

###### Binary Operator

1. `BINARY_OPERATOR_TYPE_RANGE_MATCH`: Checks if a value falls within a specified range.

##### Registering and Managing Audiences

###### Register an Audience from a Config File

Apply an audience configuration using a CLI command:

```
frogml audiences create -f <path_of_audience_config.yaml>
```

###### List All Existing Audiences

Returns a list of audience ids and names.

```
frogml audiences list
```

###### Retrieve the Details of a Specific Audience

To get the `audience-id` ,use the `frogml audiences list` command first, to retrieve all audiences with their IDs.

```
frogml audiences get --audience-id <audience_id>
```

<Callout icon="✅" theme="okay">
  **Tip**

  _**Tip**_

  For a full list of available options and parameters, use the command `frogml audiences --help`.
</Callout>

##### Directing Traffic to Audiences

In the following examples, we demonstrate how to route requests to specific Audiences.

###### Using the Python Client

```
from frog_inference import RealTimeClient

model_id = <your_model_id>
feature_vector = <...>
metadata = {"location": "new-york", "age": 25}

client = RealTimeClient(model_id=model_id)

client.predict(feature_vector, metadata=metadata)
```

###### Using REST Calls

```
curl --location --request POST 'https://models.<your_env>.qwak.ai/v1/1_hour_model/predict' \
--header 'Content-Type: application/json' \
--header 'location: new-york' \
--header 'age: 25' \
--header 'Authorization: Bearer <Auth Token>' \
--data '{
  "columns": ["feature_1", "feature_2"],
  "index": [0],
  "data": [[0.0, 0.0]]
}'
```

<Callout icon="⚠️" theme="warning">
  **Warning**

  Audience information is not stored in the JFrog ML Analytics Lake. This means that audience information of requests cannot be tracked.
</Callout>

#### Splitting Traffic with Variations

Variations are used for traffic splitting, operating above the Audience level. They have the following key characteristics:

* Each Variation is linked to a specific model Build.
* Variations always draw traffic from an Audience, so creating an Audience is required before using Variations.
* They can allocate traffic by percentage from various audiences or between different Builds, enabling effective A/B testing. Variations can be designated as Shadow Variations, which replicate a percentage of live traffic to other Builds for testing purposes.
* Each Variation can direct traffic to only one deployed Build at a time.
* The Variation assigned to a request is recorded in Analytics under the column `variation_name`.

#### Assigning Traffic to Variations

When deploying a model with multiple variations, audiences are assigned to specific variations, including the fallback audience and a fallback variation.

When distributing traffic from an audience to multiple variations, the total percentage allocated must equal 100% of that audience's traffic.

The only exception is the _Shadow Variation_, which can receive less than 100% of live traffic.

The currently deployed Variations appear under Traffic Control section in the model overview:

<Image alt="Traffic Control section in model overview" border={false} src="https://files.readme.io/19adae73594096b9ba655bd85c5e609764ce0abb8a4832d9bf205d4dc8b68177-uuid-518425b8-8751-e692-299c-d29cd5d3a2cb.png" />

**Fallback Variation:** Receives traffic that doesn't match any audience.

**Connecting Audiences to Variations:** Audiences can be linked to one or more variations, with traffic between variations distributed randomly based on defined percentages.

To modify traffic configuration:

1. Edit the deployment of the desired build.
2. Adjust the percentage of traffic for each variation.

#### Enabling Variations with the `default` Audience

In certain scenarios, you may not require the traditional traffic categorization provided by audience conditions. For such cases, JFrog ML offers support for a default audience, which lacks conditions but enables the utilization of variations for all requests.

Below is an example of configuring the default audience to enable variations:

```
api_version: v1
spec:
  audiences:
    - name: default
      description: All traffic
```

To register this audience configuration, please refer to the Audiences section above.

#### Example Deployment Request with Traffic Splitting

The following is an example of a deployment request that incorporates traffic splitting. In this example, the variation named 'test-variation' is being deployed, with traffic split evenly—50% to the default variation and 50% to the test-variation.

<Callout icon="📘" theme="info">
  **Note**

  The 'default' variation must always be deployed before any other variations. Additionally, the combined percentage of all variations must total 100%.
</Callout>

```
realtime:
  variation_name: test-variation
  audiences:
    - id: 208a9c7f-7271-416b-ac67-4939c1c45601
      name: "default" # audience name
      routes:
        - variation_name: default 
          weight: 50
          shadow: false
        - variation_name: test-variation
          weight: 50
          shadow: false
  fallback_variation: default
```

#### Undeploying a Multi-variation Realtime Model

Once you have more than one build deployed, when you undeploy an existing build, you must specify how to split the traffic after the undeployment.

###### Undeploying Models Using the UI

To undeploy a variation:

1. In the build view, click the options icon next to a deployed build and select **Undeploy**.
2. Redistribute the traffic between the remaining variations and then click **Undeploy**.

###### Undeploying Models Using the CLI

To undeploy a model with variation from the CLI, run the following command:

```
frogml models undeploy \
    --model-id <model-id> \
    --variation-name <variation-name> \
    --from-file <config-file-path>
```

With a configuration file as follows:

```
realtime:
  variation_name: <The variation name being undeployed>
   audiences:
    - id: <remaining audience id>
      name: "<audience name>
      routes:
        - variation_name: <other existing variation>
          weight: 80
          shadow: false
        - variation_name: <other existing variation 2>
          weight: 20
          shadow: false
  fallback_variation: <other existing variation>
```

If you use `--variation-name` in the CLI command, you don't have to pass the `variation_name` in the configuration file.

When undeploying from 2 variations to one, you don't have to pass any variation-related data -all the traffic will pass to the remaining variation.

<Callout icon="📘" theme="info">
  **Note**

  By default the undeploy command is executed asynchronously, which means that the command does not wait for the undeployment to complete.

  To execute the command in sync, use the `--sync` flag.
</Callout>

### Shadow Deployment

#### What is Shadow Deployment?

Shadow deployment is a special kind of deployment. The traffic is not divided between the deployments but instead multiplied. The shadow deployment itself does not respond to the request but processes it and logs the output.

This kind of deployment is best for cases where you want to check how a model behaves without affecting the actual production traffic.

#### Technical Considerations

Like a regular variation, you can configure the percentage of traffic that the deployment handles. For example, entering `20` in the percentage of the variation copies and routes every 5th request to the shadow deployment.

<Callout icon="📘" theme="info">
  **Note**

  Traffic for shadow deployments is routed from the general traffic and not from a specific variation.
</Callout>

###### Shadow Deployment in the UI

Every audience can have at most one shadow variation!

<Image alt="Shadow Deployment UI" border={false} src="https://files.readme.io/65b1f7404d63bf81a65fad4fd99dfb1d4639ef9da9e22ae553c480b84746398c-uuid-6186de32-84d8-fc3e-1a04-90bdfb4fe8ab.png" />

To add a shadow deployment:

1. Select **Traffic Control** button and check the ghost icon next to the wanted variation in the tab.
2. Specify the percentage of traffic handled by the shadow deployment model.

#### Shadow Variation using the CLI

Making the variation a shadow variation is simple. In the deployment config just add the shadow flag:

```
realtime:
  variation_name: <shadow variation name>
  audiences:
    - id: <audience_id>
      name: <audience_name>
      routes:
        - variation_name: <variation>
          weight: 100
          shadow: false
        - variation_name: <shadow variation name>
          weight: 20
          shadow: true
  fallback_variation: <variation>
```

```
frogml models deploy realtime --model-id <model-identifier> --build-id <build-id> --variation-name <shadow variation name> --from-file <config-file-path>
```

<Callout icon="📘" theme="info">
  **Note**

  The percentage of all the variations must add up to 100, regardless of shadow deployments.
</Callout>

### Protected Variations

Protected variations restrict sensitive model deployments to authorized users, ensuring only admins and maintainers can update or undeploy the model.

<Callout icon="📘" theme="info">
  **Note**

  Protected variations are supported for <Anchor label="Real-time model deployments" title="Real-Time Deployments" href="/docs/real-time-deployments">Real-time model deployments</Anchor> only on hybrid deployments.
</Callout>

#### Roles and Permissions

JFrog ML supports three user roles: **Administrators**, **Maintainers** and **Members**.

By setting a model variation as protected, you ensure that both **admins** and **maintainers** will be able to deploy protected deployments.

**Members** will only be able to view deployment details, without modifying any deployments.

#### Defining Protected Variations Via UI

When deploying a real time mode you may set the model variation as protected by using the `Protected Variation` checkbox.

By setting a deployment as protected, you can make sure only admins and maintainers will be able to modify it.

#### Defining Protected Variations Via SDK

Starting `frogml-cli 1.1`, you may use the `--protected` flag when deploying a real time model to set the model variation as protected.

```
frogml models deploy realtime --model-id "my-model-id" --variation-name "default" --protected
```

#### Deploying Multiple Variations

When deploying multiple model variations, it is possible to define some variations as protected and some as unprotected.

The protected variations will be modified only by maintainers or admins, while the unprotected variations may be modified by all users.

<Callout icon="📘" theme="info">
  **Note**

  Updating traffic split configuration is allowed only for maintainers or admins when one of the deployed variations is protected.
</Callout>

#### Multi-Environment Setup

If a model is deployed across several environments within a single JFrog ML account, _Protected Variations_ are implemented individually for each environment. For instance, consider having both _production_ and _staging_ environments, with a model deployable to each. It is possible to designate the protected variation exclusively to one of these environments. Consequently, only **Admins** and **Maintainers** would have the authority to modify the deployment in the production environment, whereas the staging environment remains accessible to all account **Members**.

## Performance and Runtime Configuration

Optimize real-time models for performance and maximal efficiency.

#### Optimizing real-time models

When deploying real-time models to JFrog ML, we receive many questions regarding performance and optimization:

* How should I configure my real-time inference endpoint during a deployment?
* Which configuration options matter the most?
* How many workers should I choose to best scaling?

We summarized in this document some of the common issues and topics to help you answer these pressing questions. 🤔

#### Cluster configuration

**Number of replicas** is the number of instances deployed in Kubernetes.

The load balancer splits the traffic between the number of replicas, the bigger the number, the more live replicas are deployed.

#### Pod configuration

* **Instance size** determines the number of vCPUs, RAM memory and GPU specifications of each replica.
* **Number of workers** determines the number of forked processes within each replica.
* **Maximal batch size** is the number of rows in the `DataFrame` received in the model's predict function.

#### When should I increase the number of replicas?

* When expecting a spike in traffic increasing the number of replicas temporarily
* When using a large number of cheaper pods.
* When modifying configuration in for a single replica doesn't increase performance when traffic increases

#### When should I increase the number of vCPUs?

* If you want to use more workers and handle multiple requests in parallel.

<Callout icon="❗️" theme="error">
  **Important**

  _**Don't waste vCPUs!**_

  If you do not increase the number of workers, but increase vCPUs, you will waste resources! Those additional CPUs will not be used.
</Callout>

In general, ML inference is a CPU-bound process, so we should follow the rule of having **1 vCPU per two worker processes**. Of course, if you run a simple model, you may try increasing the number of workers per vCPU.

#### When should I increase the number of workers?

Increase the number of workers if you need to handle more traffic and your pods still have some unused CPU capacity and RAM.

#### When should I increase the amount of RAM?

* When increasing the number of workers on each pod.

Every worker runs as a separate forked process, so there is no shared memory. In every worker, you have to load the inference service and the model.

#### When should I use a GPU for inference?

* When you have increased the max batch size per prediction request, you constantly send enough data to fill the entire batch and your CPUs don't keep up anymore.

<Callout icon="❗️" theme="error">
  **Important**

  _**Do not waste GPUs!**_

  Don't deploy a GPU instance if you process requests one by one. GPUs exist to parallelize the computation. When you process a batch of size 1, a GPU won't give you any performance improvements.
</Callout>

#### When should I increase the batch size?

* If your code in the predict function and the model can handle more than one value (preferably without iterating over them in the predict function).
* If you can group requests into batches (you have enough data to send and the client application can handle that).

## Local Deployment

Starting with SDK version 0.5.63, JFrog ML now supports local realtime deployment of models. This feature allows developers to deploy models directly on their local machine for testing and development purposes, offering an immediate and practical way to interact with the model in a realtime environment. This documentation provides a step-by-step guide on how to deploy your model locally.

#### Prerequisites

Before proceeding with the local deployment, ensure you have the following requirements met:

**FrogML SDK Version**: Ensure your system has FrogML version 1.1.61 or later installed. This version introduces support for local realtime deployment.

**Docker**: A running Docker daemon on your local machine is required. The local deployment process leverages Docker to create a containerized environment for the model. _In addition, the_`docker` _Python package is also required._

#### Deploying Your Model Locally

To deploy your model locally, follow the steps outlined below:

Run the Deployment Command: Execute the following command to deploy your model locally. Replace `<YOUR_MODEL>` with the actual model ID you wish to deploy.

```
frogml models deploy realtime --model-id "<YOUR_MODEL>" --build-id "<YOUR_BUILD_ID>" --local
```

This command initiates the deployment process by creating a Docker container in which your model will be hosted.

<Callout icon="📘" theme="info">
  **Note**

  _**Ensure Docker is Running**_

  The local deployment process requires an active Docker daemon. You can check Docker's status by running `docker info` or `docker ps` in a new terminal window. If Docker is not running, start it through your system's preferred method before attempting to deploy your model again.
</Callout>
