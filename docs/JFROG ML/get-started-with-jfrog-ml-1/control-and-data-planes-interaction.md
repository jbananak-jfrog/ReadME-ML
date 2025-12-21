---
title: Control and Data Planes
excerpt: Learn how the control and data planes of JFrog ML interact.
deprecated: false
hidden: false
metadata:
  title: Control and Data Planes Interaction
  description: >-
    The interaction between the control plane and data plane is best illustrated
    through common MLOps workflows.
  legacyUUIDs:
    - UUID-6afc4266-8eab-c86c-55f3-dab3b6aaeea2
    - UUID-b25ff6d4-a332-0f48-7aa1-0682b379b4ff
  robots: index
---
The interaction between the control plane and the data plane is best illustrated through common MLOps workflows.

## Model Build & Scan

1. **Build Trigger:** A user, via the JFrog ML CLI or SDK, initiates a jfrog ml build command. The request is authenticated by the control plane, which creates a unique build ID and records the initial metadata.
2. **Code Upload:** The control plane provides a secure, pre-signed URL for the user's client to upload the model source code directly to a staging area within the data plane.
3. **Build & Scan Execution:** The data plane picks up the job, builds the model code into a containerized artifact, and runs predefined tests. Critically, it then invokes JFrog Xray to scan the resulting artifact and its dependencies for security vulnerabilities.
4. **Store in Artifactory:** Upon a successful and clean scan, the data plane pushes the versioned, immutable model artifact to its designated repository in JFrog Artifactory.
5. **Build Status Update:** The data plane reports the final status (including the Artifactory path and Xray scan results) back to the control plane, which updates the model's metadata, making it available for deployment.

## Model Deployment

The following are the stages of model deployment:

1. **Deployment Request:** A user requests to deploy a specific model version (for example, model-a:1.2.0). The control plane verifies the user's permissions and checks that the requested model version exists and has passed all required security gates.
2. **Deploy Command:** The control plane issues a secure command to the data plane, instructing it to deploy the validated model version.
3. **Deployment Execution:** The data plane pulls the specific, versioned model artifact directly from JFrog Artifactory. It then provisions the necessary resources and deploys the model as a scalable inference endpoint.
4. **Deployment Status Update:** The data plane continuously reports the deployment's health, endpoint URL, and replica count back to the control plane, providing a live operational view to the user.

## Autoscaling ML Endpoint

This example ensures that model endpoints remain performant and cost-efficient under variable load without manual intervention.

1. **Monitor Metrics:**The data plane continuously monitors real-time performance metrics for each deployed model, such as request rate (RPS), P95 latency, error rates, and CPU/GPU utilization.
2. **Scale Autonomously:**Based on pre-defined autoscaling policies, the data plane autonomously adjusts the endpoint's resources. This can involve scaling out by adding replicas to handle traffic spikes or scaling in by removing them during idle periods to optimize costs.
3. **Report State:** The data plane reports the new scaling state, current replica count, and overall health summary back to the control plane. This ensures that the platform's UI and API always reflect the endpoint's real-time operational status.

## Feature Store Execution

This workflow automates the computation and refreshing of feature sets for training and inference.

1. **Trigger Execution:** Based on a schedule, an event, or a manual API request, the control plane initiates a feature set computation job.
2. **Execute Job:** The control plane delegates the task to the data plane, which runs the feature engineering logic (for example, joins, transformations, aggregations) using its secure compute resources.
3. **Store Results:** The data plane saves the computed feature values to the Feature Store's storage layer, making them available for low-latency retrieval by training jobs or inference services.
4. **Update Status:** The data plane reports the job's completion status, health, and execution metrics (for example, rows processed, errors) back to the control plane for central tracking and observability.

## Control and Data Plane Separation

This architectural separation is a deliberate design choice and is highly important. It provides significant advantages for security, scalability, and governance.

* **Enhanced Security & Data Sovereignty:** Your proprietary models, sensitive training data, and intellectual property never leave the Data Plane. In a hybrid deployment, this means your assets remain within your cloud account's security perimeter, satisfying strict data sovereignty and compliance requirements (for example, GDPR, HIPAA).
* **Governance & Audit:** All actions are initiated and tracked through the control plane, creating a single, auditable source of truth for the entire ML lifecycle. You can see who built, scanned, and deployed every model, enabling clear governance and simplifying compliance checks.
* **Scalability & Performance:** The execution-focused data plane can be scaled independently of the management layer. It can be geographically located close to your data sources or end-users to minimize latency and optimize performance, without affecting the central control plane.
* **Operational Efficiency:** This architecture unifies the MLOps experience. Your teams interact with a single, coherent platform for all tasks, from experimentation to production monitoring. This eliminates the complexity and security gaps associated with stitching together disparate tools for model storage, security scanning, deployment, and monitoring.
