---
title: Connect Your IDE
excerpt: To use your installed MCP gateway, instruct your IDE to use it.
deprecated: false
hidden: false
metadata:
  robots: index
---
Either:

* **Run the Magic Link (Optional):** The installation script may provide a "Magic Link" that attempts to auto-configure your IDE.

OR

* **Manual Configuration (Reliable Method):** If the link doesn't work, manually edit your IDE's MCP config file (for example, .cursor/mcp.json or VS Code settings):

```json
{
  "mcpServers": {
    "mcp-gateway": {
      "command": "jf",
      "args": ["mcp-gateway", "run"],
      "type": "stdio"
    }
  }
}
```
