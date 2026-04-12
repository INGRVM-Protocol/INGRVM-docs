# INGRVM MCP Setup Guide (Laptop to PC Handoff)

Use this guide to synchronize your MCP server configurations on your 1080 Ti PC.

## 1. Context7 MCP
This allows the agent to pull up-to-date documentation for any library.

**Configuration for `C:\Users\jparr\.gemini\settings.json`:**
```json
"context7": {
  "command": "npx.cmd",
  "args": [
    "-y",
    "@upstash/context7-mcp@latest"
  ],
  "env": {
    "CONTEXT7_API_KEY": "your_ctx7sk_key_here"
  }
}
```

## 2. GitHub MCP
This allows the agent to directly manage your repositories (create PRs, update files, etc.).

**Configuration:**
```json
"github": {
  "command": "npx.cmd",
  "args": [
    "-y",
    "@modelcontextprotocol/server-github"
  ],
  "env": {
    "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_ACTUAL_GITHUB_TOKEN_HERE"
  }
}
```

## 3. Important Notes for Windows
- **npx vs npx.cmd:** On many Windows machines, the CLI requires `"npx.cmd"` instead of just `"npx"`. I have updated the snippets above to use `.cmd`.
- **Restart:** You **must** completely close and restart the Gemini CLI app after saving `settings.json`.
- **JSON Validator:** If the servers don't show up, check your `settings.json` for missing commas or trailing braces.

---
**Status:** I am ready to initiate the LAN test from the laptop side once the PC is hosting.
