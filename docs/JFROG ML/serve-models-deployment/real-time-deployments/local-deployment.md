---
title: Local Deployment
excerpt: Supporting local realtime deployment of models
deprecated: false
hidden: false
metadata:
  robots: index
---
Starting from SDK version 0.5.63, JFrog ML supports local realtime deployment of models. This feature allows developers to deploy models directly on their local machine for testing and development purposes, offering an immediate and practical way to interact with the model in a realtime environment. 

This documentation provides a step-by-step guide on how to deploy your model locally.

## Prerequisites

Before proceeding with the local deployment, ensure you meet the following requirements:

* **FrogML SDK Version**: FrogML version 1.1.61 or later installed. This version introduces support for local realtime deployment.
* **Docker**: You have a running Docker daemon on your local machine. The local deployment process leverages Docker to create a containerized environment for the model. _In addition, the_`docker` _Python package is also required._

## Deploying Your Model Locally

▶ **To deploy your model locally:**

Run the deployment command (`deploy`): Execute the following command to deploy your model locally. Replace `<YOUR_MODEL>` with the actual model ID you wish to deploy.

```
frogml models deploy realtime --model-id "<YOUR_MODEL>" --build-id "<YOUR_BUILD_ID>" --local
```

This command initiates the deployment process by creating a Docker container in which your model will be hosted.

<Callout icon="📘" theme="info">
  _**Ensure Docker is Running**_

  The local deployment process requires an active Docker daemon. You can check Docker's status by running `docker info` or `docker ps` in a new terminal window. 

  If Docker is not running, start it through your system's preferred method before attempting to deploy your model again.
</Callout>

<br />
