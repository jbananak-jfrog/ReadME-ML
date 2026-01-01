---
title: Performance and Runtime Configuration
deprecated: false
hidden: false
metadata:
  robots: index
---
<br />

Optimize real-time models for performance and maximal efficiency.

## Optimizing Real-time Models

When deploying real-time models to JFrog ML, we receive many questions regarding performance and optimization:

* How should I configure my real-time inference endpoint during a deployment?
* Which configuration options matter the most?
* How many workers should I choose to best scaling?

We summarized in this document some of the common issues and topics to help you answer these pressing questions. 🤔

## Cluster Configuration

**Number of replicas** is the number of instances deployed in Kubernetes.

The load balancer splits the traffic between the number of replicas, the bigger the number, the more live replicas are deployed.

## Pod Configuration

* **Instance size** determines the number of vCPUs, RAM memory and GPU specifications of each replica.
* **Number of workers** determines the number of forked processes within each replica.
* **Maximal batch size** is the number of rows in the `DataFrame` received in the model's predict function.

## FAQs

<Accordion title="When Should I increase the number of replicas?" icon="fa-info-circle">
  * When expecting a spike in traffic increasing the number of replicas temporarily
  * When using a large number of cheaper pods.
  * When modifying configuration in for a single replica doesn't increase performance when traffic increases
</Accordion>

<Accordion title="When should I increase the number of vCPUs?" icon="fa-info-circle">
  * If you want to use more workers and handle multiple requests in parallel.

  <Callout icon="❗️" theme="error">
    **Important**

    ***Don't waste vCPUs!***

    If you do not increase the number of workers, but increase vCPUs, you will waste resources! Those additional CPUs will not be used.
  </Callout>

  In general, ML inference is a CPU-bound process, so we should follow the rule of having **1 vCPU per two worker processes**. Of course, if you run a simple model, you may try increasing the number of workers per vCPU.
</Accordion>

<Accordion title="When should I increase the number of workers?" icon="fa-info-circle">
  Increase the number of workers if you need to handle more traffic and your pods still have some unused CPU capacity and RAM.
</Accordion>

<Accordion title="When should I increase the amount of RAM?" icon="fa-info-circle">
  * When increasing the number of workers on each pod.

  Every worker runs as a separate forked process, so there is no shared memory. In every worker, you have to load the inference service and the model.
</Accordion>

<Accordion title=" When should I use a GPU for inference?" icon="fa-info-circle">
  * When you have increased the max batch size per prediction request, you constantly send enough data to fill the entire batch and your CPUs don't keep up anymore.

  <Callout icon="❗️" theme="error">
    **Important**

    ***Do not waste GPUs!***

    Don't deploy a GPU instance if you process requests one by one. GPUs exist to parallelize the computation. When you process a batch of size 1, a GPU won't give you any performance improvements.
  </Callout>
</Accordion>

<Accordion title="When should I increase the batch size?" icon="fa-info-circle">
* If your code in the predict function and the model can handle more than one value (preferably without iterating over them in the predict function).
* If you can group requests into batches (you have enough data to send and the client application can handle that).
</Accordion>

**When Should I Increase the Number of Replicas?**

* When expecting a spike in traffic increasing the number of replicas temporarily
* When using a large number of cheaper pods.
* When modifying configuration in for a single replica doesn't increase performance when traffic increases

**When should I increase the number of vCPUs?**

* If you want to use more workers and handle multiple requests in parallel.

<Callout icon="❗️" theme="error">
  **Important**

  _**Don't waste vCPUs!**_

  If you do not increase the number of workers, but increase vCPUs, you will waste resources! Those additional CPUs will not be used.
</Callout>

In general, ML inference is a CPU-bound process, so we should follow the rule of having **1 vCPU per two worker processes**. Of course, if you run a simple model, you may try increasing the number of workers per vCPU.

**When should I increase the number of workers?**

Increase the number of workers if you need to handle more traffic and your pods still have some unused CPU capacity and RAM.

**When should I increase the amount of RAM?**

* When increasing the number of workers on each pod.

Every worker runs as a separate forked process, so there is no shared memory. In every worker, you have to load the inference service and the model.

**When should I use a GPU for inference?**

* When you have increased the max batch size per prediction request, you constantly send enough data to fill the entire batch and your CPUs don't keep up anymore.

<Callout icon="❗️" theme="error">
  **Important**

  _**Do not waste GPUs!**_

  Don't deploy a GPU instance if you process requests one by one. GPUs exist to parallelize the computation. When you process a batch of size 1, a GPU won't give you any performance improvements.
</Callout>

**When should I increase the batch size?**

* If your code in the predict function and the model can handle more than one value (preferably without iterating over them in the predict function).
* If you can group requests into batches (you have enough data to send and the client application can handle that).
