---
title: Instance Sizes & ML Credits
excerpt: >-
  Instance sizes enable the simple selection of the best compute and memory
  resources when building and deploying models.
deprecated: false
hidden: false
metadata:
  title: Instance Sizes & ML Credits
  description: >-
    Instance sizes enable the simple selection of the best compute and memory
    resources when building and deploying models.
  legacyUUIDs:
    - UUID-53f419a9-895b-32cd-335b-f20302a480f8
    - UUID-368f6752-9956-5fb0-bcc8-02473f0ac1d0
  robots: index
---
On this page, you will find detailed information about the different instance sizes available on JFrog ML, helping you choose the optimal instance size to suit your needs.

<Callout icon="📘" theme="info">
  **Note**

  Please note that as of February 2025, we've updated our data cluster sizes and ML Credits to reflect upgrades to next-gen instances, providing faster runtimes and greater efficiency.
</Callout>

<Image alt="Select an instance size from a wide variety of options" border={false} src="https://files.readme.io/291ff7f2790ceb85c5fdbb6d7b704f62bf3987301fd415b01531ea92b0e9ab9e-uuid-4ba037ac-da87-52f2-cc89-a75d978ad32d.gif" />

## Build & Deploy Models

<Callout icon="📘" theme="info">
  **Note**

  Instance configuration for building and deploying models may still be customized individually.
</Callout>

### General Purpose Instances

JFrog ML offers a wide range of instance size to build and deploy models. Our general-purpose instances provide varying levels of CPU and memory resources, allowing you to optimize efficiency and performance.

Choose the instance size that best matches your requirements from the table below:

| Instance | CPUs | Memory (GB) | ML Credits (per hour) |
| :------- | :--- | :---------- | :-------------------- |
| Tiny     | 1    | 2           | 0.25                  |
| Small    | 2    | 8           | 0.5                   |
| Medium   | 4    | 16          | 1                     |
| Large    | 8    | 32          | 2                     |
| XLarge   | 16   | 64          | 4                     |
| 2XLarge  | 32   | 128         | 8                     |
| 4XLarge  | 64   | 256         | 16                    |

### GPU Instances

Build and deploy models on GPU-based machines from the selection available in the below table:

| Instance     | GPU Type    | GPUs | CPUs | Memory (GB) | ML Credits (per hour) |
| :----------- | :---------- | :--- | :--- | :---------- | :-------------------- |
| gpu.a10.xl   | NVIDIA A10G | 1    | 3    | 14          | 5.03                  |
| gpu.a10.2xl  | NVIDIA A10G | 1    | 7    | 28          | 6.06                  |
| gpu.a10.4xl  | NVIDIA A10G | 1    | 15   | 59          | 8.12                  |
| gpu.a10.8xl  | NVIDIA A10G | 1    | 32   | 123         | 12.24                 |
| gpu.a10.12xl | NVIDIA A10G | 4    | 47   | 189         | 28.36                 |
| gpu.t4.xl    | NVIDIA T4   | 1    | 3    | 14          | 2.19                  |
| gpu.t4.2xl   | NVIDIA T4   | 1    | 7    | 28          | 3.32                  |
| gpu.t4.4xl   | NVIDIA T4   | 1    | 15   | 59          | 5.58                  |
| gpu.a100.xl  | NVIDIA A100 | 1    | 11   | 78          | 15.9                  |
| gpu.a100.8xl | NVIDIA A100 | 8    | 95   | 1072        | 163.2                 |
| gpu.v100.xl  | NVIDIA V100 | 1    | 7    | 56          | 15.9                  |
| gpu.v100.4xl | NVIDIA V100 | 4    | 31   | 227         | 63.6                  |
| gpu.v100.8xl | NVIDIA V100 | 8    | 63   | 454         | 127.2                 |
| gpu.k80.xl   | NVIDIA K80  | 1    | 3    | 56          | 4.6                   |
| gpu.k80.8xl  | NVIDIA K80  | 8    | 31   | 454         | 36.8                  |
| gpu.k80.16xl | NVIDIA K80  | 16   | 63   | 681         | 73.8                  |
| gpu.l4.xl    | NVIDIA L4   | 1    | 3    | 12          | 3.53                  |

## Feature Store

### Data Cluster Sizes

Our Feature Store offers a variety of sizes to accommodate your needs. Select the appropriate data cluster size to ensure scalability and efficiency in handling your data ingestion jobs.

Take a look at the table below to explore the available data cluster sizes:

| Size     | ML Credits (per hour) | Notes                            |
| :------- | :-------------------- | :------------------------------- |
| Nano     | 4                     | Available for Streaming features |
| Small    | 8                     |                                  |
| Medium   | 15                    |                                  |
| Large    | 30                    |                                  |
| X-Large  | 60                    |                                  |
| 2X-Large | 120                   |                                  |

## Instance Sizes in `flogml-cli`

Using the `frogml-cli` provides you with flexibility in choosing instance sizes for building and deploying models.

Take a look at the examples below to understand how to specify the desired instance size.

### Build Models on CPU Instances

```
frogml models build --model-id "example-model-id" --instance medium .
```

### Build Models on GPU Instances

```
frogml models build --model-id "example-model-id" --instance "gpu.t4.xl" .
```

### Deploy Models on CPU Instances

```
frogml models deploy realtime --model-id "example-model-id" --instance large
```

### Deploy Models on GPU Instances

```
frogml models deploy realtime --model-id "example-model-id" --instance "gpu.a10.4xl"
```

<Callout icon="📘" theme="info">
  **Note**

  Existing resource configuration flags are supported as well: `--memory`, `--cpus`, `--gpu-type`, `--gpu-amount`.
</Callout>

## Instances Sizes in the UI

In the JFrog ML UI, you can easily select and configure instance sizes for your models. Whether you need CPU or GPU instances, our UI offers intuitive options to choose the right size for your workload.

During the deployment process, use the dropdown to specify the instance size for optimal performance.

<Image alt="The instance size dropdown offers a wide selection of available instances" border={false} src="https://files.readme.io/9f9b031bccbe0d341900f9e5a1d094a63eabf6069961758961cf677df1f1346c-uuid-7390d979-0e43-86be-41eb-d3b241cecbcf.gif" />

## Setting Custom Configuration

JFrog ML allows you to manually set custom instance configuration sizes for building and deploying your models, regardless of the default instance type options.

Custom instance type configuration is currently available for CPU deployments only.

<Image alt="Set custom instance configuration for CPU deployments" border={false} src="https://files.readme.io/f9db731b02a5a51843ced0d19d33bd564e73950155e3f557c7222a48333ec293-uuid-46a5d4d7-3652-849c-7a84-e75fe4316e80.gif" />
