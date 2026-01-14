---
title: Configure Tool Policies
deprecated: false
hidden: false
metadata:
  robots: index
---
Approval grants access to a server, but **Policies** control exactly what that server is allowed to do.

**What this step does:** It enables you to define granular rules (using Regex patterns) to strictly allow or block specific tools within a server.

**Why do this?** To prevent "Over-Privileged AI." You might trust a database server to `read_data`, but you don't want it to `drop_tables`. Policies ensure that your AI agents operate strictly within safe boundaries.

Policies are defined at the **Project level** within the server's configuration page.

**To define a policy:**

1. Navigate to the project on your Registry page.
2. Click the Policy icon next to a specific server.
3. Choose your Policy Strategy:
   * **Option A: Allow All Tools**

     **Behavior:** All tools currently provided by this server, and any added in future updates, are automatically approved.

     **Best For:** Low-risk tools or trusted internal servers.
   * **Option B: Manual Tool Control (Regex)**

     **Behavior:** You define specific Regex patterns to Allow or Deny tools.

     **Allow List:** Only tool names matching the pattern are accessible.
     * Example: `^get_.*` (Allows `get_user`, `get_log`; implicitly blocks `delete_user`).
     **Deny List:** Tool names matching the pattern are blocked.
     * Example: `._delete._` (Blocks `delete_table`, `delete_file`).

<br />
