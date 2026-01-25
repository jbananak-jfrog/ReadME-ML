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

JFrog sets up the network requirements and deploys your model on a managed Kubernetes cluster, enabling you to leverage auto-scaling and security. JFrog ML also adds a suite of monitoring tools, simplifying the process of managing your model performance.

<Image alt="Real-time model deployment overview" border={false} src="https://files.readme.io/c22e14438f25fe2ee4e3b4a00a29a8e5206e9356f795e0290e3d6251393c2c4e-uuid-5a77810a-0d93-ea20-6692-73b7b87032e3.png" />

## Deploying Real-time Models from the UI

<Image alt="Deploying a real-time model from the UI" border={false} src="https://files.readme.io/c37504c48ecaf10085959181c6692b60a42ebe123e004704f8af89c3c79dff23-uuid-ba434fe8-6697-f02d-b1e0-c884dee29695.gif" />

**To deploy a real-time model from the UI:**

1. Select **Models** in the left navigation bar
2. Select a project and a model.
3. Select the **Builds** tab.
4. Choose a build and click **Deploy**.
5. Select **Realtime** and then click **Next**.
6. Configure your real-time deployment by selecting the instance type and the initial number of replicas.
7. The **Advanced settings** configuration include additional options such as environment variables, invocation timeouts, and more.

## Deploying Real-time Models from the CLI

To deploy a model in real-time mode from the CLI, populate the following command template:

```shell
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

For example, for the model built in the [JFrog ML Quick Start](/docs/get-started-with-jfrog-ml) section, the deployment command is:

```shell
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

  The deployment command is executed asynchronously by default and does not wait for the deployment to complete. To execute the command synchronously use the `--sync` flag.
</Callout>

## Deploying a Real-time Model Using GPUs

Realtime models can be deployed on GPU instances, simply by selecting a [GPU Instance](/docs/instance-sizes-ml-credits#gpu-instances) from the available options.

```shell
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

```json IAM Policy
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

```shell
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

```shell
frogml models deploy realtime \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --local
```

<Callout icon="📘" theme="info">
  **Note**

  Deploying models locally is only available for locally generated builds using the `--no-remote` flag.
</Callout>

## Real-time Model Inference

Once you have successfully deployed a real-time model, you can use the JFrog ML Inference SDK to make predictions with it.

This example shows to invoke a model using the [Python SDK](/docs/calling-model-endpoints#python-sdk). You can install the SDK with the following command:

```shell
pip install frogml-inference
```

Note that the model inference parameters are specific to the model you are using.

In the following example, we assume that the model has already been built and deployed successfully as a real-time endpoint, and its model ID is`iris_classifier`:

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

**Summary of code components:**

* **Model Class:** `IrisClassifier` is created by inheriting from `FrogMlModel`.
* **Initialization:** The **init** method sets the `gamma` parameter for the SVM and initializes the model variable to `None`.
* **Build Method:** The `build` method loads the iris dataset, trains the SVM model using the data, and stores the trained model.
* **Predict Method:** The `predict` method takes a DataFrame as input and uses the trained model to generate predictions, returning these in a new DataFrame format.

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

To attach a new auto-scaling to a running model:

1. **Create a config file:**

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

2. **Run the following command:**

```shell
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

<br />

<br />

<br />

<br />
