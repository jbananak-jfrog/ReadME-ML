---
title: Role-based Access Control (RBAC) in JFrog ML
excerpt: For AI/ML
deprecated: false
hidden: true
metadata:
  robots: index
---
The JFrog Platform is a unified DevOps solution that provides end-to-end management and control of your software supply chain. With the introduction of Role-Based Access Control (RBAC) for JFrog ML, JFrog extends this robust security model to the AI lifecycle, enabling you to manage and govern access to your AI/ML assets with the same precision and control as you do for your software artifacts.

This document focuses on the permissions specific to JFrog ML resources. For a comprehensive understanding of managing roles and groups in the JFrog Platform, refer to the existing documentation linked in the Integrating with the JFrog Platform section.

## Key Concepts: Resources and Permissions

In JFrog ML, all assets are treated as resources that require specific permissions for you to perform actions on them. This ensures that only authorized users can access, modify, or deploy critical AI assets. The key resource types are:

* **Models:** The core AI models.
* **Model Repositories:** The repositories where models are stored.
* **Feature sets:** Collections of features used for model training and serving.
* **Feature set Repositories:** Repositories for feature sets.
* **Data Sources:** The source of data used for features.
* **Secrets:** Credentials and other sensitive information.
* **Runtime Environments:** The environments where models are deployed and executed.

Specific actions on these resources, such as building a model or querying a featureset, require a corresponding permission to be assigned to a user's role.

<br />

## User Experience: Permission-based Behavior

When you lack the necessary permission for an action, the JFrog Platform's UI, SDK, and CLI provide precise feedback to guide you.

* **UI Behavior:** All actions and views remain visible and interactive regardless of your current permission scope, however:
  * **Action Execution:** If you try to perform a restricted action (e.g., Build a model or Deploy a model), the operation will fail immediately.
  * **Data Access:** If you attempt to navigate to or load a restricted view (e.g., Model Logs), the data will not be loaded.
    In both cases, the UI displays a standard error notification to inform you of the restriction: "You do not have permission to perform this action. Please consult with your JFrog Administrator."
* **SDK and CLI Behavior:** When you attempt to run a command without the required permission, the API request returns a clear, specific error message. For example: "You do not have permission to perform this action. Please consult with your JFrog Administrator."

## Permissions Matrix for JFrog ML Resources

The permissions matrix for JFrog ML can be found on the [Manage Global Roles](/docs/manage-global-roles) page, together with all JFrog's other RBAC permissions.
