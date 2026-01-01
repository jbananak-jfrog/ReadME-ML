---
title: Local Deployment
deprecated: false
hidden: false
metadata:
  robots: index
---
Starting with SDK version 0.5.63, JFrog ML now supports local realtime deployment of models. This feature allows developers to deploy models directly on their local machine for testing and development purposes, offering an immediate and practical way to interact with the model in a realtime environment. This documentation provides a step-by-step guide on how to deploy your model locally.

## Prerequisites

Before proceeding with the local deployment, ensure you have the following requirements met:

**FrogML SDK Version**: Ensure your system has FrogML version 1.1.61 or later installed. This version introduces support for local realtime deployment.

**Docker**: A running Docker daemon on your local machine is required. The local deployment process leverages Docker to create a containerized environment for the model. _In addition, the_`docker` _Python package is also required._

## Deploying Your Model Locally

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
