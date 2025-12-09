---
title: "Instance Sizes & ML Credits"
deprecated: false
hidden: false
metadata:
  title: "Instance Sizes & ML Credits"
  description: Instance sizes enable the simple selection of the best compute and memory resources when building and deploying models.
  robots: index
  legacyUUIDs:
    - UUID-53f419a9-895b-32cd-335b-f20302a480f8
    - UUID-368f6752-9956-5fb0-bcc8-02473f0ac1d0
---
Instance sizes enable the simple selection of the best compute and memory resources when building and deploying models.

On this page, you will find detailed information about the different instance sizes available on JFrog ML, helping you choose the optimal instance size to suit your needs.

<Callout icon="📘" theme="info">
**Note**

Please note that as of February 2025, we've updated our data cluster sizes and ML Credits to reflect upgrades to next-gen instances, providing faster runtimes and greater efficiency.
</Callout>

![Select an instance size from a wide variety of options](https://files.readme.io/291ff7f2790ceb85c5fdbb6d7b704f62bf3987301fd415b01531ea92b0e9ab9e-uuid-4ba037ac-da87-52f2-cc89-a75d978ad32d.gif)

### Build & Deploy Models

<Callout icon="📘" theme="info">
**Note**

Instance configuration for building and deploying models may still be customized individually.
</Callout>


#### General Purpose Instances

JFrog ML offers a wide range of instance size to build and deploy models. Our general-purpose instances provide varying levels of CPU and memory resources, allowing you to optimize efficiency and performance.

Choose the instance size that best matches your requirements from the table below:



<Table>
  <thead>
    <tr>
      <th>
        Instance
      </th>
      <th>
        CPUs
      </th>
      <th>
        Memory (GB)
      </th>
      <th>
        ML Credits (per hour)
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        Tiny
      </td>
      <td>
        1
      </td>
      <td>
        2
      </td>
      <td>
        0.25
      </td>
    </tr>
    <tr>
      <td>
        Small
      </td>
      <td>
        2
      </td>
      <td>
        8
      </td>
      <td>
        0.5
      </td>
    </tr>
    <tr>
      <td>
        Medium
      </td>
      <td>
        4
      </td>
      <td>
        16
      </td>
      <td>
        1
      </td>
    </tr>
    <tr>
      <td>
        Large
      </td>
      <td>
        8
      </td>
      <td>
        32
      </td>
      <td>
        2
      </td>
    </tr>
    <tr>
      <td>
        XLarge
      </td>
      <td>
        16
      </td>
      <td>
        64
      </td>
      <td>
        4
      </td>
    </tr>
    <tr>
      <td>
        2XLarge
      </td>
      <td>
        32
      </td>
      <td>
        128
      </td>
      <td>
        8
      </td>
    </tr>
    <tr>
      <td>
        4XLarge
      </td>
      <td>
        64
      </td>
      <td>
        256
      </td>
      <td>
        16
      </td>
    </tr>
  </tbody>
</Table>



#### GPU Instances

Build and deploy models on GPU-based machines from the selection available in the below table:



<Table>
  <thead>
    <tr>
      <th>
        Instance
      </th>
      <th>
        GPU Type
      </th>
      <th>
        GPUs
      </th>
      <th>
        CPUs
      </th>
      <th>
        Memory (GB)
      </th>
      <th>
        ML Credits (per hour)
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        gpu.a10.xl
      </td>
      <td>
        NVIDIA A10G
      </td>
      <td>
        1
      </td>
      <td>
        3
      </td>
      <td>
        14
      </td>
      <td>
        5.03
      </td>
    </tr>
    <tr>
      <td>
        gpu.a10.2xl
      </td>
      <td>
        NVIDIA A10G
      </td>
      <td>
        1
      </td>
      <td>
        7
      </td>
      <td>
        28
      </td>
      <td>
        6.06
      </td>
    </tr>
    <tr>
      <td>
        gpu.a10.4xl
      </td>
      <td>
        NVIDIA A10G
      </td>
      <td>
        1
      </td>
      <td>
        15
      </td>
      <td>
        59
      </td>
      <td>
        8.12
      </td>
    </tr>
    <tr>
      <td>
        gpu.a10.8xl
      </td>
      <td>
        NVIDIA A10G
      </td>
      <td>
        1
      </td>
      <td>
        32
      </td>
      <td>
        123
      </td>
      <td>
        12.24
      </td>
    </tr>
    <tr>
      <td>
        gpu.a10.12xl
      </td>
      <td>
        NVIDIA A10G
      </td>
      <td>
        4
      </td>
      <td>
        47
      </td>
      <td>
        189
      </td>
      <td>
        28.36
      </td>
    </tr>
    <tr>
      <td>
        gpu.t4.xl
      </td>
      <td>
        NVIDIA T4
      </td>
      <td>
        1
      </td>
      <td>
        3
      </td>
      <td>
        14
      </td>
      <td>
        2.19
      </td>
    </tr>
    <tr>
      <td>
        gpu.t4.2xl
      </td>
      <td>
        NVIDIA T4
      </td>
      <td>
        1
      </td>
      <td>
        7
      </td>
      <td>
        28
      </td>
      <td>
        3.32
      </td>
    </tr>
    <tr>
      <td>
        gpu.t4.4xl
      </td>
      <td>
        NVIDIA T4
      </td>
      <td>
        1
      </td>
      <td>
        15
      </td>
      <td>
        59
      </td>
      <td>
        5.58
      </td>
    </tr>
    <tr>
      <td>
        gpu.a100.xl
      </td>
      <td>
        NVIDIA A100
      </td>
      <td>
        1
      </td>
      <td>
        11
      </td>
      <td>
        78
      </td>
      <td>
        15.9
      </td>
    </tr>
    <tr>
      <td>
        gpu.a100.8xl
      </td>
      <td>
        NVIDIA A100
      </td>
      <td>
        8
      </td>
      <td>
        95
      </td>
      <td>
        1072
      </td>
      <td>
        163.2
      </td>
    </tr>
    <tr>
      <td>
        gpu.v100.xl
      </td>
      <td>
        NVIDIA V100
      </td>
      <td>
        1
      </td>
      <td>
        7
      </td>
      <td>
        56
      </td>
      <td>
        15.9
      </td>
    </tr>
    <tr>
      <td>
        gpu.v100.4xl
      </td>
      <td>
        NVIDIA V100
      </td>
      <td>
        4
      </td>
      <td>
        31
      </td>
      <td>
        227
      </td>
      <td>
        63.6
      </td>
    </tr>
    <tr>
      <td>
        gpu.v100.8xl
      </td>
      <td>
        NVIDIA V100
      </td>
      <td>
        8
      </td>
      <td>
        63
      </td>
      <td>
        454
      </td>
      <td>
        127.2
      </td>
    </tr>
    <tr>
      <td>
        gpu.k80.xl
      </td>
      <td>
        NVIDIA K80
      </td>
      <td>
        1
      </td>
      <td>
        3
      </td>
      <td>
        56
      </td>
      <td>
        4.6
      </td>
    </tr>
    <tr>
      <td>
        gpu.k80.8xl
      </td>
      <td>
        NVIDIA K80
      </td>
      <td>
        8
      </td>
      <td>
        31
      </td>
      <td>
        454
      </td>
      <td>
        36.8
      </td>
    </tr>
    <tr>
      <td>
        gpu.k80.16xl
      </td>
      <td>
        NVIDIA K80
      </td>
      <td>
        16
      </td>
      <td>
        63
      </td>
      <td>
        681
      </td>
      <td>
        73.8
      </td>
    </tr>
    <tr>
      <td>
        gpu.l4.xl
      </td>
      <td>
        NVIDIA L4
      </td>
      <td>
        1
      </td>
      <td>
        3
      </td>
      <td>
        12
      </td>
      <td>
        3.53
      </td>
    </tr>
  </tbody>
</Table>



### Feature Store

#### Data Cluster Sizes

Our Feature Store offers a variety of sizes to accommodate your needs. Select the appropriate data cluster size to ensure scalability and efficiency in handling your data ingestion jobs.

Take a look at the table below to explore the available data cluster sizes:



<Table>
  <thead>
    <tr>
      <th>
        Size
      </th>
      <th>
        ML Credits (per hour)
      </th>
      <th>
        Notes
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        Nano
      </td>
      <td>
        4
      </td>
      <td>
        Available for Streaming features
      </td>
    </tr>
    <tr>
      <td>
        Small
      </td>
      <td>
        8
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        Medium
      </td>
      <td>
        15
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        Large
      </td>
      <td>
        30
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        X-Large
      </td>
      <td>
        60
      </td>
      <td>
      </td>
    </tr>
    <tr>
      <td>
        2X-Large
      </td>
      <td>
        120
      </td>
      <td>
      </td>
    </tr>
  </tbody>
</Table>



### Instance Sizes in `flogml-cli`

Using the `frogml-cli` provides you with flexibility in choosing instance sizes for building and deploying models.

Take a look at the examples below to understand how to specify the desired instance size.

#### Build Models on CPU Instances

```
frogml models build --model-id "example-model-id" --instance medium .
```

#### Build Models on GPU Instances

```
frogml models build --model-id "example-model-id" --instance "gpu.t4.xl" .
```

#### Deploy Models on CPU Instances

```
frogml models deploy realtime --model-id "example-model-id" --instance large
```

#### Deploy Models on GPU Instances

```
frogml models deploy realtime --model-id "example-model-id" --instance "gpu.a10.4xl"
```

<Callout icon="📘" theme="info">
**Note**

Existing resource configuration flags are supported as well: `--memory`, `--cpus`, `--gpu-type`, `--gpu-amount`.
</Callout>


### Instances Sizes in the UI

In the JFrog ML UI, you can easily select and configure instance sizes for your models. Whether you need CPU or GPU instances, our UI offers intuitive options to choose the right size for your workload.

During the deployment process, use the dropdown to specify the instance size for optimal performance.

![The instance size dropdown offers a wide selection of available instances](https://files.readme.io/9f9b031bccbe0d341900f9e5a1d094a63eabf6069961758961cf677df1f1346c-uuid-7390d979-0e43-86be-41eb-d3b241cecbcf.gif)

### Setting Custom Configuration

JFrog ML allows you to manually set custom instance configuration sizes for building and deploying your models, regardless of the default instance type options.

Custom instance type configuration is currently available for CPU deployments only.

![Set custom instance configuration for CPU deployments](https://files.readme.io/f9db731b02a5a51843ced0d19d33bd564e73950155e3f557c7222a48333ec293-uuid-46a5d4d7-3652-849c-7a84-e75fe4316e80.gif)
