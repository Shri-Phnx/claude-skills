# Claude Skills — by Shrinivas Ramaprasad

A growing library of reusable Claude skills built on structured prompt frameworks.
Each skill is a self-contained `SKILL.md` file that teaches Claude how to handle a specific class of tasks with precision and consistency.

---

## Skill Index

| Skill | Path | Framework / Type | Purpose |
|---|---|---|---|
| rtcfrr-prompt-builder | `skills/rtcfrr-prompt-builder/` | R·T·C·F·R·R | Build structured AI prompts for any professional deliverable — CV, LinkedIn, outreach, CareerForge |
| json-prompt-builder | `skills/json-prompt-builder/` | R·T·C·S·F·R·E·V | Build JSON prompts for AI workflows, n8n nodes, and automation pipelines |
| n8n+claude_Web/Desktop | `skills/n8n+claudedesktop/` | Conversational Guide | Step-by-step guide for connecting n8n to Claude — covers both Claude.ai Web (no software needed) and Claude Desktop (mcp-remote). Includes OS-specific config paths, token setup, workflow publishing checklist, and troubleshooting. |
| canvas-design | `skills/canvas-design/` | Visual Art | Create beautiful posters, art, and static visual designs as .png and .pdf using design philosophy methodology |
| mcp-builder | `skills/mcp-builder/` | Dev Guide | Build high-quality MCP servers in Python (FastMCP) or TypeScript that enable LLMs to interact with external APIs |
| skill-creator | `skills/skill-creator/` | Meta Skill | Create, iterate, test, and optimise new Claude skills from scratch — includes eval framework and description optimisation |
| uae-cv-builder | `skills/uae-cv-builder/` | Executive Resume | Build a premium UAE/GCC ATS-optimised executive resume from an uploaded CV — tailored for Program Manager, PMO, IT Director, and Service Delivery roles in Dubai, Abu Dhabi, and the broader GCC market |
| skeptical-vc-red-team | `skills/skeptical-vc-red-team/` | Red Team Framework | Universal skeptical VC red team for any product idea, business concept, feature, or brainstorm — diagnoses risks, guides toward the right path, provides actionable suggestions, and simplifies complex challenges into clear next steps |

---

## Folder Structure

```
claude-skills/
└── skills/
    └── [skill-name]/
        ├── SKILL.md                   ← Claude skill definition (frontmatter + instructions)
        └── assets/                    ← Supporting files (templates, references)
            └── [asset-file]
```

---

## How to Install a Skill

1. Download the `SKILL.md` from the skill folder
2. Go to Claude → Settings → Skills → Add skill
3. Upload the `SKILL.md` file
4. Claude will automatically trigger it based on the description in the frontmatter

---

## Frameworks Used

- **R·T·C·F·R·R** — Role · Task · Context · Format · Rules · Review (universal prompt framework)
- **R·T·C·S·F·R·E·V** — Extended for JSON / structured output tasks
- **Conversational Guide** — Plain text step-by-step guide delivered conversationally in chat
- **Visual Art** — Design philosophy methodology for museum-quality visual output
- **Dev Guide** — Step-by-step development framework for MCP server creation
- **Meta Skill** — Skills about creating and optimising other skills
- **Executive Resume** — UAE/GCC executive ATS resume builder with recruiter scanning optimisation
- **Red Team Framework** — Universal skeptical VC pressure-testing framework for product ideas and brainstorms

---

*Maintained by Shrinivas Ramaprasad | Built for CareerForge automations, n8n workflows, and AI agent pipelines.*
