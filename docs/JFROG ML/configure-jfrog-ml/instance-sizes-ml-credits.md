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

![Select an instance size from a wide variety of options](https://files.readme.io/291ff7f2790ceb85c5fdbb6d7b704f62bf3987301fd415b01531ea92b0e9ab9e-uuid-4ba037ac-da87-52f2-cc89-a75d978ad32d.gif)

## Build & Deploy Models

JFrog ML offers a wide range of instance size to build and deploy models.

<Callout icon="📘" theme="info">
  **Note**

  Instance configuration for building and deploying models may still be customized individually.
</Callout>

### General Purpose Instances

Our general-purpose instances provide varying levels of CPU and memory resources, allowing you to optimize efficiency and performance.

Select the instance size that best matches your requirements from the table below:

| Instance ID | display_name | display_order | CPUs | Memory (GB) | Enabled | GPU   | cluster_type |
| :---------- | :----------- | :------------ | :--- | :---------- | :------ | :---- | :----------- |
| prompt      | Prompt       | 1             | 0.5  | 1           | true    | 0.125 | SAAS         |
| tiny        | Tiny         | 2             | 1.0  | 2           | true    | 0.25  | SAAS         |
| small       | Small        | 3             | 2.0  | 4           | true    | 0.5   | SAAS         |
| medium      | Medium       | 4             | 4.0  | 8           | true    | 1.0   | SAAS         |
| large       | Large        | 5             | 8.0  | 16          | true    | 2.0   | SAAS         |
| xlarge      | XLarge       | 6             | 16.0 | 32          | true    | 4.0   | SAAS         |
| 2xlarge     | 2XLarge      | 7             | 32.0 | 64          | true    | 8.0   | SAAS         |
| 4xlarge     | 4XLarge      | 8             | 64.0 | 128         | true    | 16.0  | SAAS         |

### GPU Instances

Build and deploy models on GPU-based machines from the selection available in the below table:

| Instance ID          | Display Name | Display Order | CPU  | Memory Amount (GB) | GPU Amount | GPU Type                   | AWS Supported | GCP Supported | qpu        | Enabled | Cluster Type | Azure Supported |
| -------------------- | ------------ | ------------- | ---- | ------------------ | ---------- | -------------------------- | ------------- | ------------- | ---------- | ------- | ------------ | --------------- |
| gpu.azure.m60.4xl    | M60 4XLarge  | 3             | 47.0 | 443                | 4          | NVIDIA_M60                 | false         | false         | 22.8       | true    | SAAS         | true            |
| gpu.azure.m60.2xl    | M60 2XLarge  | 2             | 23.0 | 219                | 2          | NVIDIA_M60                 | false         | false         | 11.4       | true    | SAAS         | true            |
| gpu.azure.m60.xl     | M60 XLarge   | 1             | 11.0 | 107                | 1          | NVIDIA_M60                 | false         | false         | 5.7        | true    | SAAS         | true            |
| gpu.azure.a10.xl     | A10 XLarge   | 11            | 71.0 | 875                | 1          | NVIDIA_A10                 | false         | false         | 32.6       | true    | SAAS         | true            |
| gpu.azure.a10.large  | A10 Large    | 10            | 35.0 | 435                | 1          | NVIDIA_A10                 | false         | false         | 16.0       | true    | SAAS         | true            |
| gpu.azure.a10.medium | A10 Medium   | 9             | 17.0 | 215                | 1          | NVIDIA_A10                 | false         | false         | 8.0        | true    | SAAS         | true            |
| gpu.azure.a10.small  | A10 Small    | 8             | 11.0 | 105                | 1          | NVIDIA_A10                 | false         | false         | 4.54       | true    | SAAS         | true            |
| gpu.azure.t4.8xl     | T4 8XLarge   | 7             | 64.0 | 435                | 4          | NVIDIA_T4                  | false         | false         | 21.76      | true    | SAAS         | true            |
| gpu.azure.t4.4xl     | T4 4XLarge   | 6             | 15.0 | 105                | 1          | NVIDIA_T4                  | false         | false         | 6.02       | true    | SAAS         | true            |
| gpu.azure.t4.2xl     | T4 2XLarge   | 5             | 7.0  | 51                 | 1          | NVIDIA_T4                  | false         | false         | 3.76       | true    | SAAS         | true            |
| gpu.azure.t4.xl      | T4 XLarge    | 4             | 3.0  | 23                 | 1          | NVIDIA_T4                  | false         | false         | 2.63       | true    | SAAS         | true            |
| gpu.a10.12xl         | A10 12Xlarge | 15            | 47.0 | 189                | 4          | NVIDIA_A10G                | true          | false         | 28.3600006 | true    | SAAS         | false           |
| gpu.a10.2xl          | A10 2Xlarge  | 2             | 7.0  | 28                 | 1          | NVIDIA_A10G                | true          | false         | 6.05999994 | true    | SAAS         | false           |
| gpu.a10.4xl          | A10 4Xlarge  | 3             | 15.0 | 59                 | 1          | NVIDIA_A10G                | true          | false         | 8.11999989 | true    | SAAS         | false           |
| gpu.a10.8xl          | A10 8Xlarge  | 4             | 31.0 | 123                | 1          | NVIDIA_A10G                | true          | false         | 12.2399998 | true    | SAAS         | false           |
| gpu.a10.xl           | A10 Xlarge   | 1             | 3.0  | 14                 | 1          | NVIDIA_A10G                | true          | false         | 5.03000021 | true    | SAAS         | false           |
| gpu.a100.8xl         | A100 8Xlarge | 8             | 95.0 | 1072               | 8          | NVIDIA_A100                | true          | false         | 163.199997 | true    | SAAS         | false           |
| gpu.gcp.a100.8xl     | A100 8Xlarge | 8             | 95.0 | 1072               | 8          | NVIDIA_A100_80GB_8_96_1360 | false         | true          | 163.199997 | true    | SAAS         | false           |
| gpu.gcp.t4.2xl       | T4 2Xlarge   | 6             | 7.0  | 25                 | 1          | NVIDIA_T4_1_8_30           | false         | true          | 3.31999993 | true    | SAAS         | false           |
| gpu.gcp.t4.4xl       | T4 4Xlarge   | 7             | 15.0 | 52                 | 1          | NVIDIA_T4_1_16_60          | false         | true          | 5.57999992 | true    | SAAS         | false           |
| gpu.gcp.t4.xl        | T4 Xlarge    | 5             | 3.0  | 11                 | 1          | NVIDIA_T4_1_4_15           | false         | true          | 2.19000006 | true    | SAAS         | false           |
| gpu.l4.xl            | L4 Xlarge    | 17            | 3.0  | 12                 | 1          | NVIDIA_L4                  | true          | false         | 3.52999997 | true    | SAAS         | false           |
| gpu.t4.2xl           | T4 2Xlarge   | 6             | 7.0  | 28                 | 1          | NVIDIA_T4                  | true          | false         | 3.31999993 | true    | SAAS         | false           |
| gpu.t4.4xl           | T4 4Xlarge   | 7             | 15.0 | 59                 | 1          | NVIDIA_T4                  | true          | false         | 5.57999992 | true    | SAAS         | false           |
| gpu.t4.xl            | T4 Xlarge    | 5             | 3.0  | 14                 | 1          | NVIDIA_T4                  | true          | false         | 2.19000006 | true    | SAAS         | false           |
| gpu.v100.4xl         | V100 4Xlarge | 10            | 31.0 | 227                | 4          | NVIDIA_V100                | true          | false         | 63.5999985 | true    | SAAS         | false           |
| gpu.v100.8xl         | V100 8Xlarge | 11            | 63.0 | 454                | 8          | NVIDIA_V100                | true          | false         | 127.199997 | true    | SAAS         | false           |
| gpu.a100.xl          | A100 Xlarge  | 16            | 10.0 | 75                 | 1          | NVIDIA_A100                | true          | false         | 15.8999996 | true    | SAAS         | false           |
| gpu.v100.xl          | V100 Xlarge  | 9             | 7.0  | 53                 | 1          | NVIDIA_V100                | true          | false         | 15.8999996 | true    | SAAS         | false           |
| gpu.gcp.v100.xl      | V100 Xlarge  | 9             | 7.0  | 52                 | 1          | NVIDIA_V100_1_8_52         | false         | true          | 15.8999996 | true    | SAAS         | false           |
| gpu.gcp.v100.4xl     | V100 4Xlarge | 10            | 31.0 | 208                | 4          | NVIDIA_V100_4_32_208       | false         | true          | 63.5999985 | true    | SAAS         | false           |

## Feature Store

### Data Cluster Sizes

Our Feature Store offers a variety of data cluster sizes to accommodate your needs. Select the appropriate size to ensure scalability and efficiency in handling your data ingestion jobs.

The table below explores the available data cluster sizes:

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

See the examples below to understand how to specify the required instance size.

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

  Existing resource configuration flags are also supported: `--memory`, `--cpus`, `--gpu-type`, `--gpu-amount`.
</Callout>

## Instance Sizes in the UI

In the JFrog ML UI, you can easily select and configure instance sizes for your models. Whether you need CPU or GPU instances, the JFrog ML UI offers intuitive options to choose the correct size for your workload.

During the deployment process, use the dropdown to specify the instance size for optimal performance.

![The instance size dropdown offers a wide selection of available instances](https://files.readme.io/9f9b031bccbe0d341900f9e5a1d094a63eabf6069961758961cf677df1f1346c-uuid-7390d979-0e43-86be-41eb-d3b241cecbcf.gif)

## Setting Custom Configuration

JFrog ML enables you to manually set custom instance configuration sizes for building and deploying your models, regardless of the default instance type options.

Custom instance type configuration is currently available for CPU deployments only.

![Set custom instance configuration for CPU deployments](https://files.readme.io/f9db731b02a5a51843ced0d19d33bd564e73950155e3f557c7222a48333ec293-uuid-46a5d4d7-3652-849c-7a84-e75fe4316e80.gif)
