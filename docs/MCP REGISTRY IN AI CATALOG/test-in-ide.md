---
title: Test ? in IDE
excerpt: Once the gateway is running and the IDE is configured, verify the connection.
deprecated: false
hidden: true
metadata:
  robots: index
---
To test the MCP is connected:

1. **Open your AI Chat:** 
   * Cursor: Open the "Composer" (Cmd+I or Ctrl+I).
   * VS Code: Open the GitHub Copilot Chat.
2. **Ask the Agent:** Type the following prompt:
   "Please list the MCP tools currently available to me."
   OR use the CLI list command.
3. **Success:** The agent should respond with a list of tools allowed for your project (for example, "I have access to the following tools: `sqlite`, `fetch`...").