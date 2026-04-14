# Claude Skills — by Shrinivas

A growing library of reusable Claude skills built on structured prompt frameworks.
Each skill is a self-contained `SKILL.md` file that teaches Claude how to handle a specific class of tasks with precision and consistency.

---

## Skill Index

| Skill | Path | Framework | Purpose |
|---|---|---|---|
| rtcfrr-prompt-builder | `skills/rtcfrr-prompt-builder/` | R·T·C·F·R·R | Build structured AI prompts for any professional deliverable — CV, LinkedIn, outreach, CareerForge |
| json-prompt-builder | `skills/json-prompt-builder/` | R·T·C·S·F·R·E·V | Build JSON prompts for AI workflows, n8n nodes, and automation pipelines |

---

## Folder Structure

```
claude-skills/
└── skills/
    └── [skill-name]/
        ├── SKILL.md                    ← Claude skill definition
        └── [supporting-templates].md  ← Reference files
```

---

## How to Install a Skill

1. Download the `SKILL.md` from the skill folder
2. Upload it to your Claude skills directory
3. Claude will automatically trigger it based on the description in the frontmatter

---

## Frameworks Used

- **R·T·C·F·R·R** — Role · Task · Context · Format · Rules · Review (universal prompt framework)
- **R·T·C·S·F·R·E·V** — Extended for JSON/structured output tasks

---

*Maintained by Shrinivas | Built for CareerForge automations, n8n workflows, and AI agent pipelines.*
