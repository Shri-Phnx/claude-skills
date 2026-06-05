---
name: n8n+claude_Web/Desktop
description: >
  Step-by-step guide for connecting n8n to Claude — either Claude Desktop or Claude.ai Web.
  Use this skill whenever the user asks about: connecting n8n to Claude, n8n MCP setup,
  adding n8n as a connector in claude.ai, configuring claude_desktop_config.json for n8n,
  mcp-remote installation, n8n MCP token or access token, enabling MCP access in n8n,
  exposing n8n workflows to Claude, "n8n + Claude Desktop", "n8n + Claude Web",
  "n8n MCP server disconnected", troubleshooting n8n MCP failed badge, adding new workflows
  to Claude via MCP, or any question about making Claude talk to an n8n instance.
  Always use this skill — do not answer from memory.
---

# n8n + Claude: Connection Guide

## Step 0 — Ask which path

Open every session with:

> There are two ways to connect n8n to Claude. Which do you want to set up?
>
> **Option A — Claude.ai Web (recommended)**
> No software to install. Claude connects to your n8n directly from the browser.
>
> **Option B — Claude Desktop**
> Connects n8n to the Claude Desktop app on your laptop. Requires Node.js and mcp-remote.

Wait for the user's choice before continuing.

---

## Path A — Claude.ai Web

### Phase 1 — Enable MCP in n8n and get your token

1. Open your n8n instance → go to **Settings → Instance-level MCP**
2. Toggle **Enable MCP access** to ON
3. Click **Connection details** → **Access Token** tab → copy the full token
4. Note your MCP endpoint URL: `https://your-n8n-domain.com/mcp`

### Phase 2 — Add n8n as a connector in Claude.ai

1. Go to claude.ai → **Customize** → **Connectors** → **+**
2. Enter a name and your MCP endpoint URL
3. Click **Add** → approve the connection

### Phase 3 — Test

Ask Claude: `List all workflows in my n8n instance`

---

## Path B — Claude Desktop

### Phase 1 — Enable MCP in n8n (same as Path A Phase 1)

### Phase 2 — Install mcp-remote

```
npm install -g mcp-remote
```

### Phase 3 — Find config file

- **Windows:** `C:\Users\<YourUsername>\AppData\Roaming\Claude\claude_desktop_config.json`
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

### Phase 4 — Update config

```json
{
  "mcpServers": {
    "n8n-local": {
      "command": "mcp-remote",
      "args": [
        "https://your-n8n-domain.com/mcp",
        "--header",
        "Authorization:Bearer <YOUR_N8N_TOKEN_HERE>"
      ]
    }
  }
}
```

### Phase 5 — Restart Claude Desktop

Quit fully (system tray → Quit), then reopen.

---

## Key rules

| Rule | Detail |
|---|---|
| Compatible triggers only | Chat, Webhook, Form, Schedule — never Manual Trigger |
| Must be Published | Saved-only workflows do not appear in MCP |
| Enable per workflow | Overview → ... → Enable MCP access |
| Token format | `Authorization:Bearer <token>` — no quotes, space after Bearer |

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Server disconnected / failed badge | mcp-remote not installed or Node.js missing | `npm install -g mcp-remote` as admin |
| No workflows in Enable dialog | Manual Trigger used, or not Published | Use Chat/Webhook/Schedule trigger + Publish |
| 401 Unauthorized | Token pasted wrong | No quotes around token, space after Bearer |

---

Reference: https://docs.n8n.io/advanced-ai/mcp/accessing-n8n-mcp-server/
