---
title: Batch Deployments
excerpt: Given a successful build, you can deploy your model as a batch application.
deprecated: false
hidden: false
metadata:
  title: Batch Deployments
  description: Given a successful build, you can deploy your model as a batch application.
  legacyUUIDs:
    - UUID-75a82642-5be8-5e27-1489-aee42b6f6880
    - UUID-12d4c0e7-3bd3-f679-1303-c306f58f29df
    - UUID-d07a4461-8a25-0063-eaea-f2499c725049
    - UUID-25499b2d-d497-1322-368b-8be210e34367
    - UUID-e83ba9c3-bcef-b30a-5e5e-2d8d47d8abcc
    - UUID-17525b81-e83e-b999-cd33-7cbbe6cb9084
    - UUID-a2de47d6-19af-2414-56bd-574a3c34ed8e
    - UUID-38aa3c57-6473-a6a8-ed98-84a92a2332fc
  robots: index
---
This deployment type enables you to run batch inference executions in the system, and handle data files from an online cloud storage provider.

## Deployment Configuration

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Parameter
      </th>

      <th style={{ textAlign: "left" }}>
        Description
      </th>

      <th style={{ textAlign: "left" }}>
        Default Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Model ID [**Required**]
      </td>

      <td style={{ textAlign: "left" }}>
        The Model ID, as displayed on the model header.
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Build ID [**Required**]
      </td>

      <td style={{ textAlign: "left" }}>
        The JFrog ML-assigned build ID.
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Initial number of pods
      </td>

      <td style={{ textAlign: "left" }}>
        The number of <Anchor label="k8s pods" target="_blank" href="https://kubernetes.io/docs/concepts/workloads/pods/">k8s pods</Anchor> to be used by the deployment. Each pod handles one or more files/tasks.
      </td>

      <td style={{ textAlign: "left" }}>
        1
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        CPU fraction
      </td>

      <td style={{ textAlign: "left" }}>
        The CPU fraction allocated to each pod. The CPU resource is measured in CPU units. One CPU, in JFrog ML, is equivalent to:

        1 AWS vCPU

        1 GCP Core

        1 Azure vCore

        1 Hyperthread on a bare-metal Intel processor with Hyperthreading
      </td>

      <td style={{ textAlign: "left" }}>
        2
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Memory
      </td>

      <td style={{ textAlign: "left" }}>
        The RAM memory (in MB) to allocate to each pod.
      </td>

      <td style={{ textAlign: "left" }}>
        512
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        IAM role ARN
      </td>

      <td style={{ textAlign: "left" }}>
        The user-provided AWS custom IAM role.
      </td>

      <td style={{ textAlign: "left" }}>
        None
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        GPU Type
      </td>

      <td style={{ textAlign: "left" }}>
        The GPU Type to use in the model deployment. Supported options are, NVIDIA K80, NVIDIA Tesla V100, NVIDIA T4 and NVIDIA A10.
      </td>

      <td style={{ textAlign: "left" }}>
        None
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        GPU Amount
      </td>

      <td style={{ textAlign: "left" }}>
        The number of GPUs available for the model deployment.

        Varies based on the selected GPU type.
      </td>

      <td style={{ textAlign: "left" }}>
        Based on GPU Type
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Purchase Option
      </td>

      <td style={{ textAlign: "left" }}>
        Choose between `on-demand` or `spot` instances for the batch executions.
      </td>

      <td style={{ textAlign: "left" }}>
        None (spot)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Service Account Key Secret Name
      </td>

      <td style={{ textAlign: "left" }}>
        The service account key secret name to reach Google cloud services.
      </td>

      <td style={{ textAlign: "left" }}>
        None
      </td>
    </tr>
  </tbody>
</Table>

## Batch Deployment from the UI

To deploy a batch model from the UI:

1. In the JFrog Platform, select **Models** and select a model to deploy.
2. Select the **Builds** tab. Find a build to deploy and click the deployment toggle. The **Deploy** dialog box appears.
3. Select **Batch** and then select **Next**.

## Batch Deployment from the CLI

To deploy a model in batch mode from the CLI, populate the following command template:

```
frogml models deploy batch \
    --model-id <model-id> \
    --build-id <build-id> \
    --pods <pods-count> \
    --cpus <cpus-fraction> \
    --memory <memory-size>
```

For example, for the model built in the [JFrog ML Quick Start](/docs/get-started-with-jfrog-ml) section, the deployment command is:

```
frogml models deploy batch \
    --model-id churn_model \
    --build-id 7121b796-5027-11ec-b97c-367dda8b746f \
    --pods 4 \
    --cpus 3 \
    --memory 1024
```

<br />

<br />

<br />

<br />
