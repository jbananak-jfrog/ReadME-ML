---
title: What is the MCP Catalog?
excerpt: >-
  The MCP Catalog governs the active tools those models use, giving you complete
  control over actions like "read_file" or "query_db".
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  robots: index
---
Think of the Model Context Protocol (MCP) as a "universal adapter" for your AI.

AI agents (like GitHub Copilot or Cursor) are powerful, but they live in a bubble—they don't automatically have access to your private databases or internal files. MCP bridges this gap. It is an open standard that lets you plug your AI tools into your actual systems, enabling them to safely retrieve data and execute tasks without needing a custom integration for every single service.

## Why use the JFrog MCP Catalog?

As developers rapidly adopt these tools to make their AI assistants smarter, it creates a new challenge: "Shadow AI Integrations."

Without a central catalog, developers might download unverified MCP servers from the internet, giving AI agents unchecked access to sensitive company environments. The JFrog MCP Catalog solves this by acting as your organization's secure marketplace and control plane.

| Problem            | JFrog's Solution                                                                                                                                                                                   |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Unknown Risks      | **Single Source of Truth:** Platform teams can curate a trusted list of approved MCP servers, ensuring every tool is known and accounted for.                                                      |
| Security Gaps      | **Vulnerability Scanning:** Just like any other software package, MCP servers are scanned for security vulnerabilities and license compliance before they are approved.                            |
| Over-Privileged AI | **Granular Control:** Instead of giving an AI tool total access, you can enforce **Project-Based Policies**. You can allow an AI to read from a database but strictly block it from deleting data. |

<br />


