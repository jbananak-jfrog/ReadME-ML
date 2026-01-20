---
title: Install the MCP Gateway
deprecated: false
hidden: true
metadata:
  robots: index
---
The MCP gateway is responsible for the interactions between your MCP server and JFrog’s AI catalog.

To use MCP tools, you  must run a project-scoped script that installs the JFrog CLI (if it is not already installed), and  the mcp-gateway gateway plugin, and configures your local environment to talk to the correct JFrog Project.

1. Log in to the JFrog Platform and navigate to AI/ML > MCP Servers.
2. Select your Project from the dropdown menu (e.g., my-project).
3. Click Set Me Up to generate your custom installation script.
4. Run the command in your terminal. Choose the method that matches your authentication preference:

   **Option A: SSO / Interactive Login (Recommended)** - Use this if you want to log in via your browser.
   ```
   curl -sL https://get.jfrog.io/mcp | bash -s -- --url <YOUR_JPD_URL> --project <PROJECT_KEY>
   ```
   **Option B: Token-Based Authentication**  - Use this for automated setups or if you already have an Identity Token.
   ```Text bash
   curl -sL https://get.jfrog.io/mcp | bash -s -- --config <YOUR_TOKEN> --project <PROJECT_KEY>
   ```
   What this script does:
   * Installs or updates the JFrog CLI (jf).
   * Installs the mcp-gateway plugin.
   * Configures your Current Project context so you only see allowed tools.
   * Sets up Default Repositories (npm, Docker, PyPI) for secure package resolution.

<br />