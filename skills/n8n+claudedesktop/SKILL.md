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

## How to deliver this skill

Respond conversationally. Ask the user which path they want first, then walk through the
relevant steps one phase at a time. Do not dump everything at once.

---

## Step 0 — Ask which path

Open every session with:

> There are two ways to connect n8n to Claude. Which do you want to set up?
>
> **Option A — Claude.ai Web (recommended)**
> No software to install. Claude connects to your n8n directly from the browser.
> Works from any device, any time. Requires your n8n instance to be publicly accessible
> (e.g. on a VPS or cloud host).
>
> **Option B — Claude Desktop**
> Connects n8n to the Claude Desktop app on your laptop. Requires Node.js and mcp-remote
> installed locally. Only works when your laptop is on.

Wait for the user's choice before continuing.

---

## Path A — Claude.ai Web

### What this does
Claude.ai connects directly to your n8n MCP endpoint over HTTPS. No local software needed.
The connection is persistent as long as your n8n instance is running.

### Prerequisites
- n8n hosted on a publicly accessible URL (VPS, cloud host, or n8n Cloud)
- Admin or owner access to your n8n instance
- A Claude.ai account

---

### Phase 1 — Enable MCP in n8n and get your token

1. Open your n8n instance → go to **Settings → Instance-level MCP**
2. Toggle **Enable MCP access** to ON (requires admin/owner role)
3. Click **Connection details** (top-right of the page)
4. Switch to the **Access Token** tab → copy the full token immediately.
   It is redacted once you leave this page.
   The token is a long JWT starting with `eyJ...` — copy the entire string.
5. Note your MCP endpoint URL. Format: `https://your-n8n-domain.com/mcp`

> Token tip: no quotes around it, no line breaks, copy the full string.

---

### Phase 2 — Add n8n as a connector in Claude.ai

1. Go to claude.ai → click **Customize** → **Connectors**
2. Click the **+** button to add a custom connector
3. Enter a name (e.g. `n8n VPS`) and your MCP endpoint URL from Phase 1
4. Click **Add** → Claude.ai will prompt you to authorise
5. Approve the connection

---

### Phase 3 — Test the connection

Ask Claude:

> List all workflows in my n8n instance

Expected: Claude returns your workflow list. If it does, you are live.

---

### Adding new workflows (repeat every time)

1. Build with a compatible trigger: **Chat, Webhook, Form, or Schedule**. Never Manual Trigger.
2. Save → dropdown next to Save → **Publish**
3. **Overview** → find the workflow → **... (3 dots)** → **Enable MCP access**
4. **Settings → Instance-level MCP** → confirm it appears in the table
5. Optional but recommended: **... → Edit description** — describe what the workflow does.
   This helps Claude identify and use it correctly.
6. Test: ask Claude "List all workflows in my n8n instance" → it should appear.

No restart or config change needed after initial setup.

---

## Path B — Claude Desktop

### What this does
Claude Desktop connects to n8n via `mcp-remote`, a local proxy that bridges Claude's stdio
transport with n8n's HTTP endpoint. Works on Windows, macOS, and Linux.

### Prerequisites
- Claude Desktop installed
- Node.js v18 or newer (nodejs.org)
- n8n running (local, VPS, or n8n Cloud)
- Admin or owner access to n8n

---

### Phase 1 — Enable MCP in n8n and get your token

Same as Path A Phase 1. Get your token and MCP endpoint URL before continuing.

---

### Phase 2 — Install mcp-remote

Open a terminal (run as Administrator on Windows):

```
npm install -g mcp-remote
```

Success output: "added 82 packages" (or similar number).
Running `mcp-remote --version` will error — this is normal and not a problem.

Node.js v18 or newer is required. If npm is not found, download Node.js from nodejs.org first.

---

### Phase 3 — Find your config file

Locate `claude_desktop_config.json` at:

- **Windows:** `C:\Users\<YourUsername>\AppData\Roaming\Claude\claude_desktop_config.json`
  Quick: Win+R → type `%APPDATA%\Claude` → Enter
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
  Quick: Finder → Cmd+Shift+G → paste path above
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

Open in any text editor (Notepad, VS Code, etc.).

Tip: share the full content of your existing config file in chat and Claude will
produce the complete updated file for you — safer than editing manually.

---

### Phase 4 — Update the config file

Add the `n8n-local` block inside `mcpServers`. If other entries already exist, add
alongside them — do not delete existing entries.

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

Replace `https://your-n8n-domain.com/mcp` with your actual endpoint URL.
Replace `<YOUR_N8N_TOKEN_HERE>` with your full `eyJ...` token.

Token format rules:
- No quotes around the token
- Space after Bearer is required
- Full string, no line breaks

---

### Phase 5 — Restart Claude Desktop

1. Save the config file
2. Quit Claude Desktop fully — do not just close the window
3. Right-click the Claude icon in the system tray → **Quit**
4. Reopen Claude Desktop
5. Go to **Settings → Connectors** → `n8n-local` should appear without a red failed badge

---

### Phase 6 — Test the connection

Ask Claude Desktop:

> List all workflows in my n8n instance

Expected: Claude returns your workflow list using the n8n MCP tools.

---

### Adding new workflows (repeat every time)

Same checklist as Path A. Compatible trigger + Publish + Enable MCP access.
No config file change or restart needed for new workflows after initial setup.

---

## Key rules — both paths

| Rule | Detail |
|---|---|
| Compatible triggers only | Chat, Webhook, Form, Schedule — never Manual Trigger |
| Must be Published | Saved-only workflows do not appear in MCP |
| Enable per workflow | Overview → ... → Enable MCP access |
| Token format | `Authorization:Bearer <token>` — no quotes, space after Bearer |
| New workflows | No config change or restart needed after initial setup |

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Server disconnected / failed badge (Desktop) | mcp-remote not installed or Node.js missing | `npm install -g mcp-remote` as admin; Node.js v18+ required |
| Not valid MCP config (Desktop) | Wrong config format | Use mcp-remote command format, not `"type":"http"` |
| No workflows in Enable dialog | Manual Trigger used, or not Published | Use Chat/Webhook/Schedule trigger + Publish |
| New workflow not in Claude | Not published or MCP access not enabled | Publish → Overview → ... → Enable MCP access |
| 401 Unauthorized | Token pasted wrong | No quotes around token, space after Bearer, full eyJ... string |
| Failed to add connector (Web) | Already added or auth issue | Disconnect existing connector first, then re-add |
| DNS / connection error (Web) | n8n not publicly reachable | Check VPS firewall, nginx proxy buffering, DNS |
| execute_workflow fails | Workflow not Active | Ensure workflow is Published and Active |

---

## Reference

- Official n8n MCP docs: https://docs.n8n.io/advanced-ai/mcp/accessing-n8n-mcp-server/
- Repo: github.com/Shri-Phnx/claude-skills

---

## Version history

| Version | Date | Change |
|---|---|---|
| v3 | May 2026 | Full rewrite: renamed to n8n+claude_Web/Desktop, dropped HTML widget, added Claude.ai Web path, conversational delivery format |
| v2 | April 2026 | Updated triggers, Set node instructions, system tray quit, Step 7 new workflow checklist, per-step error capture |
| v1 | April 2026 | Initial release |
