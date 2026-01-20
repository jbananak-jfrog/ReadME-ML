---
title: 'Reference: CLI Commands'
deprecated: false
hidden: true
metadata:
  robots: index
---
The `jf mcp-gateway` tool allows you to manage your local your local environment and connection to the catalog.

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Command
      </th>

      <th style={{ textAlign: "left" }}>
        Syntax
      </th>

      <th style={{ textAlign: "left" }}>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Start Gateway
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway run`
      </td>

      <td style={{ textAlign: "left" }}>
        Required. Starts the local server process. This terminal must remain open for the IDE to connect.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        List Tools
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway list`
      </td>

      <td style={{ textAlign: "left" }}>
        Lists all MCP servers and tools currently approved for your active project.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Add Server
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway add <SERVER_NAME>`

        <br />
      </td>

      <td style={{ textAlign: "left" }}>
        Adds a specific approved server (e.g., sqlite) to your local configuration. This automatically removes unapproved tools and syncs new policies.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Remove Server
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway remove <SERVER_NAME>`

        <br />
      </td>

      <td style={{ textAlign: "left" }}>
        Removes a server from your local configuration.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Set Project
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway project-set <KEY>`
      </td>

      <td style={{ textAlign: "left" }}>
        Switches your active project context (see  Access Tools for a Different Project).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Inspect
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway inspect <SERVER_NAME>`

        <br />
      </td>

      <td style={{ textAlign: "left" }}>
        Shows detailed metadata, including the source URL, version, and active policies for a specific server.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Help
      </td>

      <td style={{ textAlign: "left" }}>
        `jf mcp-gateway --help`
      </td>

      <td style={{ textAlign: "left" }}>
        Displays the full list of available commands and flags.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Edit Server
      </td>

      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>
        Edit an existing server
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        List Installed MCP Servers
      </td>

      <td style={{ textAlign: "left" }}>
        `list-installed`
      </td>

      <td style={{ textAlign: "left" }}>
        Displays only the MCP servers installed on your machine
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        List Allowed MCP Servers
      </td>

      <td style={{ textAlign: "left" }}>
        `list-available`
      </td>

      <td style={{ textAlign: "left" }}>
        List allowed servers from the JFrog AI Catalog
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Initialize
      </td>

      <td style={{ textAlign: "left" }}>
        `init`
      </td>

      <td style={{ textAlign: "left" }}>
        Initialize the curation registry.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Show selected project
      </td>

      <td style={{ textAlign: "left" }}>
        `project-show`
      </td>

      <td style={{ textAlign: "left" }}>
        Show the currently selected project (from plugin settings) and all available projects for the user.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

<br />
