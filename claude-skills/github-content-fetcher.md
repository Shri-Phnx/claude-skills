---
name: github-content-fetcher
description: >
  Fetch, categorise, and save content from any GitHub repository into the correct local folder.
  Use this skill whenever the user shares a GitHub URL (repo, folder, or file) and wants the
  content downloaded and organised. Trigger on: "fetch this repo", "download this skill",
  "grab this from GitHub", "process this GitHub link", "install this", "save this skill locally",
  or any GitHub URL shared in conversation. Automatically detects whether content is a Claude
  Skill, Claude Code Skill, Plugin, n8n workflow, MCP config, script, reference doc, or other.
  Saves to the correct local folder, mirrors Claude Skills and Claude Code Skills to the
  Shri-Phnx/claude-skills GitHub repo, and logs everything to memory.md.
---

# GitHub Content Fetcher

Automatically fetches content from a GitHub URL, detects what it is, saves it to the right
local folder, mirrors skills to GitHub, and logs to memory.md. No manual sorting needed.

---

## How to Trigger This Skill

The user shares a GitHub URL in any of these forms:
- Full repo: `https://github.com/owner/repo`
- Folder: `https://github.com/owner/repo/tree/main/skills`
- Single file: `https://github.com/owner/repo/blob/main/path/to/file.md`

Extract `owner` and `repo` from the URL. If a specific path is in the URL, start browsing there.
If it is a repo root, browse from `/`.

---

## Step 1 — Parse the URL

Extract from the GitHub URL:
- `owner`: the GitHub username or org
- `repo`: the repository name
- `path`: the file or folder path (default `/` if not specified)
- `ref`: the branch (default `main`, fall back to `master` if main returns 404)

Use `get_file_contents` with these values to retrieve the content or directory listing.

---

## Step 2 — Browse and Inventory

If the path returns a directory listing, walk it recursively (max 2 levels deep to avoid
overloading on large repos). Build a flat inventory of all files with their paths.

If the path returns a single file, add just that file to the inventory.

Skip these always: `.gitignore`, `.git/`, `LICENSE`, `node_modules/`, `__pycache__/`,
`.DS_Store`, `*.lock`, `package-lock.json`.

---

## Step 3 — Classify Each File

For every file in the inventory, apply this decision logic in order. Stop at the first match.

### Claude Skill
Conditions (any one matches):
- File is named `SKILL.md`
- File is inside a folder named `skills/` or `claude-skills/`
- File is `.md` and its frontmatter contains `name:` and `description:` fields
- File description mentions: "use this skill when", "trigger when", "use when the user"

Action: fetch content, extract `name` from frontmatter (or use filename), save as
`[skill-name].md`

Save to: `C:\Users\PC\Documents\Claude\Skills\Claude Skills\[skill-name].md`
Mirror to GitHub: `claude-skills/[skill-name].md` in `Shri-Phnx/claude-skills`

### Claude Code Skill
Conditions (any one matches):
- File is inside a folder named `claude-code-skills/`
- File is a Claude Skill AND its content is primarily about: writing code, building MCP servers,
  CLI tools, development workflows, code generation, or technical implementation

Action: same as Claude Skill but different destination.

Save to: `C:\Users\PC\Documents\Claude\Skills\Claude Code skills\[skill-name].md`
Mirror to GitHub: `claude-code-skills/[skill-name].md` in `Shri-Phnx/claude-skills`

### Plugin
Conditions (any one matches):
- File extension is `.plugin` or `.skill` (zip archive)
- File is named `plugin.json` or `manifest.json` with a `plugin` key
- File is inside a folder named `plugins/`

Action: fetch and save as-is, preserving original filename.

Save to: `C:\Users\PC\Documents\Claude\Plugins\[filename]`
No GitHub mirror.

### n8n Workflow
Conditions (all must match):
- File extension is `.json`
- File content contains both `"nodes"` and `"connections"` keys at the top level

Action: fetch and save as-is, preserving original filename.

Save to: `C:\Users\PC\Documents\Claude\n8n\Workflows\[filename]`
No GitHub mirror.

### MCP Config
Conditions (any one matches):
- File content contains `"mcpServers"` key
- File is named `claude_desktop_config.json` or `mcp-config.json`

Action: fetch and save, prefixing filename with `[repo-name]-` to avoid collisions.

Save to: `C:\Users\PC\Documents\Claude\Reference\mcp-configs\[repo-name]-[filename]`
No GitHub mirror.

### Script
Conditions (any one matches):
- File extension is `.py`, `.js`, `.ts`, `.sh`, `.bash`
- File is inside a folder named `scripts/`

Action: fetch and save, preserving original filename.

Save to: `C:\Users\PC\Documents\Claude\Scripts\[filename]`
No GitHub mirror.

### Reference / Documentation
Conditions (any one matches):
- File is named `README.md`
- File is inside a folder named `docs/` or `documentation/`
- File extension is `.md` or `.txt` but does not match Claude Skill conditions

Action: fetch and save, prefixing filename with `[repo-name]-`.

Save to: `C:\Users\PC\Documents\Claude\Reference\[repo-name]-[filename]`
No GitHub mirror.

### Config / Settings
Conditions (any one matches):
- File extension is `.yaml`, `.yml`, `.toml`, `.env.example`
- File is named `config.json` (but not an MCP config)

Action: fetch and save, prefixing with `[repo-name]-`.

Save to: `C:\Users\PC\Documents\Claude\Reference\configs\[repo-name]-[filename]`
No GitHub mirror.

### Unclassifiable
Conditions: none of the above matched after reading the file.

Action: fetch and save as-is.

Save to: `C:\Users\PC\Documents\Claude\Other Gitgub content\[repo-name]-[filename]`
No GitHub mirror. Flag in the session summary.

---

## Step 4 — Fetch and Save

For each classified file:
1. Fetch full file content using `get_file_contents`
2. Write to the local path using the Write tool
3. For Claude Skills and Claude Code Skills: push to `Shri-Phnx/claude-skills` using
   `create_or_update_file` or batch via `push_files` if multiple skills

If a file already exists at the local path, check if the content differs before overwriting.
If identical, skip and note "already up to date" in the summary.

---

## Step 5 — Update memory.md

After all files are saved, append one entry to `memory.md` under Patterns and Preferences:

```
- [date]: Fetched [repo-name] from github.com/[owner]/[repo].
  Saved: [N] Claude Skills, [N] Claude Code Skills, [N] Plugins, [N] n8n Workflows,
  [N] Scripts, [N] Reference files, [N] Configs, [N] Unclassified.
  Files: [comma-separated list of saved filenames]
```

---

## Step 6 — Report to User

Deliver a short summary table at the end:

| File | Classified As | Saved To | GitHub Mirror |
|---|---|---|---|
| [filename] | Claude Skill | Claude Skills\ | Yes |
| [filename] | n8n Workflow | n8n\Workflows\ | No |
| [filename] | Unclassified | Other Gitgub content\ | No — review needed |

Flag any unclassified files explicitly so the user can decide what to do with them.

---

## Edge Cases

**Private repos from other people:** GitHub MCP cannot read repos where access has not been
granted. If `get_file_contents` returns 404 or 403, tell the user: "This repo is private or
access is not granted. Share the raw file URL directly and I will fetch and classify it."

**Single raw file URL:** If the user shares a raw.githubusercontent.com URL, fetch it with
web_fetch, classify the content, and save accordingly.

**Very large repos:** If the root directory listing contains more than 30 items, ask the user
which folder to focus on rather than trying to process the entire repo.

**Ambiguous classification:** If a file could be either a Claude Skill or a Claude Code Skill,
read the full content. If it involves writing, running, or debugging code as its primary output,
it is a Claude Code Skill. If it guides Claude in a task or conversation, it is a Claude Skill.
When genuinely uncertain after reading, default to Claude Skill and note the ambiguity in the
summary.

---

## Folder Reference (exact paths, match case)

| Content Type | Local Path |
|---|---|
| Claude Skill | `C:\Users\PC\Documents\Claude\Skills\Claude Skills\` |
| Claude Code Skill | `C:\Users\PC\Documents\Claude\Skills\Claude Code skills\` |
| Plugin | `C:\Users\PC\Documents\Claude\Plugins\` |
| n8n Workflow | `C:\Users\PC\Documents\Claude\n8n\Workflows\` |
| MCP Config | `C:\Users\PC\Documents\Claude\Reference\mcp-configs\` |
| Script | `C:\Users\PC\Documents\Claude\Scripts\` |
| Reference / Docs | `C:\Users\PC\Documents\Claude\Reference\` |
| Config / Settings | `C:\Users\PC\Documents\Claude\Reference\configs\` |
| Unclassified | `C:\Users\PC\Documents\Claude\Other Gitgub content\` |

GitHub mirror (Shri-Phnx/claude-skills):
- Claude Skills → `claude-skills/[skill-name].md`
- Claude Code Skills → `claude-code-skills/[skill-name].md`
- All other types → local only, no mirror
