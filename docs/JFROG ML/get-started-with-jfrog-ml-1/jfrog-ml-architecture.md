---
title: JFrog ML Architecture
excerpt: >-
  Explore the architecture that makes JFrog ML a single, trusted platform for
  MLOps.
deprecated: false
hidden: false
metadata:
  title: JFrog ML Architecture
  description: >-
    Explore the architecture that makes JFrog ML a single, trusted platform for
    MLOps.
  legacyUUIDs:
    - UUID-50703231-b3fc-579c-e523-eb5343b3e6df
    - UUID-fc07ad7d-464d-f2b8-86de-ea25304abbf8
  robots: index
---
At its core, the architecture leverages the power of the JFrog platform. JFrog Artifactory serves as the central, immutable model registry, providing versioning, traceability, and governance for all ML models and their dependencies. Security is embedded at every stage, with JFrog Xray performing deep scanning of models, containers, and artifacts to proactively identify vulnerabilities and license compliance issues.

Seamlessly integrating with the JFrog platform, JFrog ML enables you to use JFrog Artifactory as a trusted model registry while leveraging JFrog Security products to secure your entire model development lifecycle.

<Image alt="JFrog ML Logo" border={false} src="https://files.readme.io/581df9b88feda60fe172d526e7da2416b26d5d092552cd1e7ffdabfcb9d9a848-uuid-593a04bd-f54d-8c1b-8702-5291c546c32d.png" />

This document outlines the architecture that enables this powerful combination of flexibility and security.

## High Level Architecture

The JFrog ML architecture is fundamentally based on a separation of concerns, divided into two distinct components:

* The JFrog ML Control Plane, and
* The JFrog ML Data Plane

This design ensures that sensitive data and computational workloads remain isolated within a secure environment, while orchestration and metadata management are handled centrally.

<Image alt="JFrog ML high level overview" border={false} src="https://files.readme.io/494adbc982d4684e99bb9a7707576e6bc1cae534c6120164a85ddd64c3278503-uuid-f5a6747b-cb2c-d992-2d0b-b30d141a3471.png" />

### JFrog ML Control Plane

The Control Plane is the centralized orchestration and management layer, securely hosted and managed by JFrog. It serves as the brain of the system, coordinating all activities without ever accessing sensitive customer data or models. Its sole focus is on metadata, workflow management, and state tracking.

**Core Responsibilities:**

* **Orchestration & Metadata Management:** Manages all non-sensitive metadata for entities such as models, builds, pipelines, and deployments. This includes tracking versions, parameters, and relationships between these entities.
* **Orchestration & Workflow Management:** Coordinates multi-step workflows such as build, deploy, promote and monitor and delegates sensitive operations to the Data Plane.
* **Delegation & Status Reporting:** Sends requests to the data plane and receives operation updates (for example, build progress, deployment status).

### JFrog ML Data Plane

The Data Plane is the secure execution environment where all sensitive data processing, model computation, and artifact storage occurs. This plane is deployed either in JFrog's secure cloud (for a fully managed SaaS experience) or directly within a customer's own cloud environment when deploying JFrog ML self-hosted. 

**Core Responsibilities:**

* **Secure Execution Environment:** Executes all computational workloads, including model training jobs, build processes, and batch inference tasks.
* **Model Repository & Artifact Storage:** Stores models, artifacts, and associated metadata in encrypted registries or object stores.
* **Feature Store:** Manages the storage and retrieval of feature data for training and real-time inference.
* **Inference Lake:** Collects and stores model prediction logs, ground truth data, and operational metrics for monitoring and analysis.
* **Real-time Model Serving:** Manages the deployment of models as scalable, high-availability endpoints, complete with built-in monitoring and logging.
* **Compute & Autoscaling Management:** Provisions the necessary compute resources for all jobs and manages the autoscaling of model endpoints based on real-time traffic, latency, or custom metrics.

See <Anchor label="Control and Data Planes Interaction" target="_blank" href="/docs/control-and-data-planes-interaction">Control and Data Planes Interaction</Anchor>

<br />
