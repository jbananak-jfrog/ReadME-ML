---
title: Connect Remote MCP Servers
deprecated: false
hidden: false
icon: ❓
metadata:
  robots: index
---
While many MCP servers are downloaded and run locally (like Docker containers), others exist as live services on the web.

**What this step does:** It registers a specific URL (a Remote Endpoint) in the catalog, treating a live web service as if it were just another tool.

**Why do this?** This enables you to access powerful third-party SaaS tools or shared internal agents without having to install or run them on your own machine. By connecting to the remote endpoint, you get immediate access to the tool's capabilities through your IDE, saving you system resources and setup time.
To onboard a remote endpoint/Connect a Cloud Service/Register a Web Address ???

To _**onboard a remote endpoint/Connect a Cloud Service/Register a Web Address ???**_

<Callout icon="📘" theme="info">
  **This task requires Admin permissions.**
</Callout>

1. Navigate to **AI/ML** > **Discovery** > **MCP Servers**.
2. Click **Add Custom Server**. ???? ???? ????
3. Select **Remote Endpoint** (or External Service) as the source type.
4. Enter the connection details:
   * Server Name: A unique identifier for the catalog (e.g., acme-cloud-mcp).
   * Endpoint URL: The secure URL (https://...) where the MCP server is running (must support Server-Sent Events/SSE).
5. **Validation:** Click **Test Connection**. The platform will perform a handshake to verify the server speaks the MCP protocol.
   Click Save.
