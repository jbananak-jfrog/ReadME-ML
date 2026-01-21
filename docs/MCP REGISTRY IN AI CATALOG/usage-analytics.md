---
title: Usage & Analytics
excerpt: >-
  The platform provides built-in reporting to help Admins track adoption,
  security, and performance.
deprecated: false
hidden: true
metadata:
  robots: index
---
This section gives you visibility into how the MCP Catalog is being used across your organization and highlights potential security risks.

The primary report focuses on detecting Shadow AI — understanding which tools developers are using locally that have not been vetted or approved by the organization.

**How it works:** The local MCP Gateway compares the developer's local configuration (mcp.json) against the project's allow-list. If it finds a mismatch, it sends a telemetry event to the backend.

**Goal:** Enables administrators to see usage outside the curated list and decide whether to add, allow, or block these MCPs.

* **Adoption:** Useful for understanding if developers are actually using approved tools
  * **Trending MCP Servers:** Which AI tools are most popular across the organization right now.
  * **Active Gateways:** Number of developers actively polling the catalog.
* **Security & Risk:** Useful for understanding how often JFrog’s security policies are blocking actions:
  * **Blocked Tools:** A count of specific tools (capabilities) that were blocked by policy.
* **Performance:** Is the system slow? How long do updates take to reach developers?
  * **Policy Sync Latency:** The average time it takes for a policy change (for example, blocking a tool) to propagate to developer machines (Target: \< 1 minute).
