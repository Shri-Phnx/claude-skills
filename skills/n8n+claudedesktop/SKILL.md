---
name: n8n+claudedesktop
description: >
  Interactive step-by-step wizard for connecting n8n MCP server to Claude Desktop.
  ALWAYS use this skill when a user asks about any of the following: connecting n8n to Claude,
  n8n MCP setup, configuring claude_desktop_config.json for n8n, mcp-remote installation,
  n8n MCP token or access token, enabling MCP access in n8n, exposing n8n workflows to Claude,
  "n8n + Claude Desktop", "n8n MCP server disconnected", troubleshooting n8n MCP failed badge,
  adding a new workflow to Claude via MCP, or any question about making Claude Desktop talk
  to an n8n instance. Use this skill even if the user only mentions one of these keywords —
  do not answer from memory. This skill contains a verified interactive guide with OS-specific
  config paths, hosting-specific endpoints, per-step error capture prompts, and a Step 7
  checklist for adding new workflows after initial setup.
compatibility:
  tools_required:
    - visualize:read_me
    - visualize:show_widget
---

# n8n + Claude Desktop MCP Setup Skill

## What this skill does

Renders a personalised 7-step interactive wizard that guides users through connecting n8n
to Claude Desktop via MCP. The wizard detects OS (Windows / macOS / Linux) and n8n hosting
type (local / cloud / self-hosted VPS), then generates exact config paths, endpoint URLs,
config file snippets, and per-step error capture buttons.

---

## Execution instructions

### Step 1 — Load the visualiser module
Call `visualize:read_me` with `modules: ["interactive"]` before rendering.

### Step 2 — Render the interactive guide
Call `visualize:show_widget` using the **full HTML content** from:
`assets/interactive-guide.html`

Read that file and pass its content verbatim as `widget_code`.

Loading messages to use:
```
["Setting up your personalised guide...", "Loading all OS and hosting paths...", "Wiring step engine and error capture..."]
```
Title: `n8n_mcp_claude_desktop_guide`

### Step 3 — Support the user at each step
Remain available after rendering. The widget's "Share error or screenshot" buttons fire
pre-filled prompts into chat. Diagnose and fix immediately using the troubleshooting
reference below.

### Step 4 — Config file update requests
When a user shares their existing `claude_desktop_config.json`, provide the **complete
updated file** with the `n8n-local` block inserted — never just the snippet in isolation.
Always mask credentials with `<YOUR_TOKEN_HERE>`.

---

## Key rules — always apply

| Rule | Detail |
|---|---|
| Compatible triggers only | Chat, Webhook, Form, Schedule — never Manual Trigger |
| Must be Published | Saved-only workflows do not appear in MCP |
| Enable per workflow | Overview → ... (3 dots) → Enable MCP access |
| mcp-remote required | Bridges Claude Desktop stdio ↔ n8n HTTP |
| Token format | `Authorization:Bearer <token>` — no quotes, space after Bearer |
| Quit fully | Right-click system tray icon → Quit (not just close window) |
| New workflows | No config change or restart needed after initial setup |

---

## Step 7 — Adding new workflows checklist (repeat each time)

1. Build with compatible trigger: Chat, Webhook, Form, or Schedule
2. Ctrl+S / Cmd+S to save
3. Dropdown next to Save → Publish
4. Overview → workflow → ... (3 dots) → Enable MCP access
5. Settings → Instance-level MCP → confirm it appears in table
6. ... → Edit description (helps Claude identify the workflow)
7. Test: "List all workflows in my n8n instance"
8. No config file change or Claude Desktop restart required

---

## Troubleshooting reference

| Symptom | Cause | Fix |
|---|---|---|
| "Not valid MCP config" on startup | Used `"type":"http"` directly | Switch to `mcp-remote` command format |
| "Server disconnected" / failed badge | mcp-remote not installed | `npm install -g mcp-remote` as admin; Node.js v18+ required |
| No workflows in Enable dialog | Manual Trigger used, or not Published | Use Chat/Webhook/Schedule + Publish |
| New workflow not in Claude | Not published or MCP access not enabled | Publish → Overview → ... → Enable MCP access |
| 401 Unauthorized | Token pasted wrong | No quotes around token, space after "Bearer", full `eyJ...` string |
| n8n not reachable | Not publicly accessible | Check firewall, reverse proxy, network settings |
| Config changes not taking effect | Not quit from system tray | Right-click tray icon → Quit, then reopen |
| execute_workflow fails | Workflow not Active | Ensure workflow is Published and Active |

---

## Assets

| File | Purpose | When to read |
|---|---|---|
| `assets/interactive-guide.html` | Full HTML widget code for the setup wizard | Always — read before calling show_widget |

---

## Reference

- Official n8n MCP docs: https://docs.n8n.io/advanced-ai/mcp/accessing-n8n-mcp-server/
- Repo: github.com/Shri-Phnx/claude-skills

## Version history

| Version | Date | Change |
|---|---|---|
| v2 | April 2026 | Updated triggers, Set node instructions, system tray quit, Step 7 new workflow checklist, per-step error capture |
| v1 | April 2026 | Initial release |
