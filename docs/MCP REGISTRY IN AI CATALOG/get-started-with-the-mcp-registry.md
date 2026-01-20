---
title: 'Get Started '
excerpt: >-
  Follow these steps to install the JFrog MCP Gateway and connect your IDE to
  your organization's approved tools.
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

Once you are sure you have the required runtimes, install the MCP Registry as follows:

* [Install the MCP Gateway](/docs/install-the-mcp-gateway)
* [Connect Your IDE](/docs/connect-your-ide)
* [Start the  MCP Gateway](/docs/start-the-mcp-gateway)
* <br />

<br />

Test Your Connection to the IDE

<Callout icon="📘" theme="info">
  The installation script will check for these and warn you if they are missing, but it will not install them for you.
</Callout>
