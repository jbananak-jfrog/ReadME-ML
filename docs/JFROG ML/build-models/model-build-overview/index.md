---
title: Model Build Overview
deprecated: false
hidden: false
metadata:
  title: Model Build Overview
  description: Learn how to easily build a model on JFrog ML
  legacyUUIDs:
    - UUID-1e1717bd-a990-ef61-f73b-c88807281bc6
    - UUID-b99330dc-89d0-f415-bb3b-5394dacbcbda
    - UUID-e7749bd6-96fd-3645-085d-f553d6788e1a
    - UUID-5c446e4d-6004-628e-efe6-2532736b1eea
    - UUID-92351a63-7ff2-4dea-35a1-91e706555c62
    - UUID-3d6aba73-ad31-9fb8-9fe4-124abbd7e4a6
    - UUID-4a1ddc4e-f4fa-67bf-3ced-6b6e7ad7e987
    - UUID-a0c3f5dc-d2e5-23cd-d3f7-208d80cc9d39
    - UUID-96679a3e-16ee-5f70-9b1d-1501870d98ea
    - UUID-251bbfb6-7da7-6343-d503-444ab0e79174
  robots: index
---
Learn how to easily build a model on JFrog ML

## About Model Builds

A model build in JFrog ML is the process of creating a ready-for-deployment, trained, serialized, and tested version of your machine learning model.

During the build, JFrog ML packages your model’s source code, dependencies, and metadata into a secure, immutable artifact stored in JFrog Artifactory.

## Triggering a Build

The build process is designed to be integrated directly into your development workflow. Depending on your goal, you can trigger a build through the following methods:

**JFrog ML CLI/SDK:** Use the CLI or SDK to trigger a new build from your local environment or CI/CD pipeline. This is the primary method for creating initial model versions.

**JFrog ML UI:** The Web UI is used for managing existing builds. While you cannot trigger a brand-new build from scratch here, you can perform specific actions such as:

* **Rebuild:** Re-run a previous build configuration.
* **Promote to Build:** Transition a successful experiment (model version) into a formal build artifact.

The build runs on JFrog’s scalable infrastructure, supporting any workload size—from lightweight pre-trained models to full-scale training pipelines.

## Model Build Lifecycle

The remote build process on JFrog ML comprises the following steps:

1. Creating the model's virtual environment
2. Executing the build function
3. Running unit and integration tests
4. Serializing the model
5. Building a docker image
6. Pushing the docker image to JFrog ML model registry

## Building a Model with FrogML CLI

The following steps will show you how easily to build your first model on JFrog ML.

### Prerequisites

Before starting, you must install the required development tools:

* **[FrogML CLI](/docs/setting-up-jfrog-ml#install-frogml-cli)** Ensure the CLI is installed and configured on your machine.
* **FrogML Python SDK:** Install the FrogML Python SDK via pip:
  ```shell
   [install the FrogML CLI](/docs/setting-up-jfrog-ml#install-frogml-cli).
  ```

### 1. Creating a New Model

Start by creating a new project and model on the JFrog ML platform. Note that the command creates a remote project and model rather than local files.

Your model ID will be the model name in lowercase letters and stripped from spaces (for example, `titanic`).

```shell
frogml models create "Titanic" --project "example-models"
```

### 2. Generating the Model Code

Generate the _Titanic_ example model, from the templates provided with the FrogML SDK.

This command creates the local files necessary for the build (on JFrog ML).

```shell
frogml models init --example titanic .
```

Format: `frogml models init --example <example-name> <local-model-directory>`

### 3. Building Your Model

With the local code ready, and the remote model record created, initiate the build. Build names are unique across a project.

```shell
frogml models build --model-id titanic --name v1 ./titanic_survival_classification
```

<Callout icon="📘" theme="info">
  **Note**

  The build name parameter is optional.
</Callout>

The models build command works in the following format:

```shell
frogml models build --model-id <remote-model-id> --name <build-name> <local-model-directory>
```

Running the above command generates the build ID and a link you can follow to view the live build status:

```shell
✅ Fetching model code (0:00:00.22)
✅ Registering frogml build - 100% (0:00:05.77)

Build ID 2cac1883-47eb-44dd-9806-bdd9887dcc16 triggered remotely

########### Follow build logs in the CLI
frogml models builds logs -b 2cac1883-47eb-44dd-9806-bdd9887dcc16 --follow

########### Follow build logs in the platform
https://mydemo.jfrog.io/ui/ml/models/credit_risk/build/2cac1883-47eb-44dd-9806-bdd9887dcc16
```

### 4. Tracking Build Progress

Building a model takes several minutes to complete. To view the build status, open the the model's build table.

When the build is complete, you can proceed to deploy your model.

There are two options for viewing the build progress logs:

#### Option1: Build Page in JFrog ML UI

Visit your model's page and choose the current build ID. Alternatively, follow the link you received in the CLI.

#### Option 2: Following Logs in the CLI

```shell
frogml models builds logs -b <build-id> --follow
```

* `<build-id>` - The build ID that you received when executing the build remotely.

### Building Models with GPUs....

JFrog ML [_GPU Instances_](/docs/instance-sizes-ml-credits#deploy-models-on-gpu-instances) provide high-performance computing resources that can significantly accelerate the model build process. Easily customize your build resources to achieve faster training times and better results.

To build a model on a GPU instance, specify the following additional arguments in the model build command:

```shell
frogml models build  --model-id <model-id> --instance gpu.t4.xl .
```

<Callout icon="📘" theme="info">
  **Notes**

  * _**Choosing the Correct GPU**_

    Visit the JFrog ML [_GPU Resources_](/docs/instance-sizes-ml-credits#instances-sizes-in-the-ui) page to select the resources that best fit your use-case.

    Each GPU type has its own configuration of pre-defined memory and number of CPUs.
  * JFrog ML utilizes Spot instances for cost-effective GPU-based builds. This may result in a slightly extended wait time when initiating GPU Spot instances.
</Callout>

### Deploying Models with GPUs

<Callout icon="📘" theme="info">
  **Note**

  To deploy models using a GPU, you do not need to build it on a GPU instance.

  Simply use the `--gpu-compatible` flag during the model build process.
</Callout>

When deploying a model on a GPU instance, you must verify that the model was built using a GPU compatible image. Building a model using a GPU compatible image installs additional dependencies and drivers.

Creating a GPU compatible image is done by adding the `--gpu-compatible` flag or building your model on a GPU instance.

Running the below command will build your model on a regular CPU instance, but will enable you to later deploy it on a GPU instance.

```shell
frogml models build  --model-id <model-id> --gpu-compatible .
```

### Tagging Your Model Build

Tags can be attached to specific builds for identification and tracking.

Add model tags from JFrog ML UI manually, or add tags via the FrogML CLI:

```shell
frogml models build --model-id <model-id> -T <tag_1> -T <tag_2> <local-model-directory>
```

Use the `model-id` of the model to which you want to attach tags.

### Using Environment Variable in Model Builds

You may use and pass environment variables to your models build in the CLI using the following command:

```shell
frogml models build --model-id <model-id> -E ENV_VAR=VALUE <local-model-directory>
```

or for example with mock values:

```shell
frogml models build --model-id "titanic" -E VERSION_NUMBER=1.2 -E MODEL_NAME=catboost .
```

#### Passing Secrets as Environment Variables

JFrog ML allows passing environment variables to model builds which receive values from JFrog ML secrets during the model build process.

While secret values will be accessible as environment variables during the build, they won't be displayed in the UI alongside other passed environment variables.

To implement this, you need to supply the environment variable value in the specified format: `<key>=<secret.{secret-name}>` .

For instance, if you have an API token stored under a JFrog ML secret named `cloud_token` and wish to pass it in the build under the environment variable `APP_TOKEN`, you would utilize the following command as an example:

```shell
frogml models build --model-id <model>  -E APP_TOKEN=secret.cloud_token <dest>
```

<Callout icon="📘" theme="info">
  **Note**

  Note: Please note that the secrets must exist in the JFrog ML platform before running the above command.
</Callout>
