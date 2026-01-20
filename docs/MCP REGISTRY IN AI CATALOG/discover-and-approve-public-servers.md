---
title: Discover and Approve Public Servers
deprecated: false
hidden: true
metadata:
  robots: index
---
The MCP Catalog comes pre-loaded with an index of public tools from the official open-source ecosystem.

**What this step does:** It enables you to browse, vet, and allow open-source MCP servers for your developers.

**Why do this?** If you need standard tools (like "SQLite" or "Google Drive"). Instead of downloading unverified packages from the internet, you can use the catalog to check the tools' vulnerabilities and licenses before approving them for use in specific projects.

**To browse the MCP catalog:**

1. Navigate to **AI/ML** > **Discovery** > **MCP Servers**. This view displays all public MCP servers indexed from the official Model Context Protocol Registry.

Before enabling a tool, check if it is safe.

**To review security:**

1. Click on any server card (e.g., sqlite-mcp) to open its details.
2. Look for the **Xray Data** section.
   * **Vulnerabilities:** JFrog Xray scans the underlying Docker image or NPM/Python package to detect known CVEs.
   * **Licenses:** detailed license information ensures compliance with your organization’s legal policies.

## Approving MCP Servers for Projects

By default, public servers are blocked. To make one available:

1. Click the **Add to Allowed** button on the server card.
2. Select the **Project**(s) for which you want this tool to be available.
3. Click **Save**. The server is now listed in the "Registry" page (meaning, allowed MCP Servers) for that project. Developers running the `jf mcp-gateway` client in that project will see it immediately.