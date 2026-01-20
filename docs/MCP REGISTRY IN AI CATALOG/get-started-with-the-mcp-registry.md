---
title: 'Get Started '
excerpt: >-
  Follow these steps to install the JFrog MCP Gateway and connect your IDE to
  your organization's approved tools.How to get started using your MCP Registry
deprecated: false
hidden: true
metadata:
  robots: index
---
## Prerequisites

The MCP Gateway connects you to tools, but it does not install the underlying runtimes for them. Before proceeding, ensure your machine has the runtimes matching the tools you plan to use:

* **Node.js (npx):** Required for running NPM-based MCPs.
* **Python (uv):** Required for running Python-based MCPs.
* **Docker:** Required for running containerized MCPs.

<Callout icon="📘" theme="info">
   The installation script will check for these and warn you if they are missing, but it will not install them for you.
</Callout>