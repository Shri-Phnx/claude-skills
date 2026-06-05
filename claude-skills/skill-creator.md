---
name: skill-creator
description: Create new skills, modify and improve existing skills, and measure skill performance. Use when users want to create a skill from scratch, edit, or optimize an existing skill, run evals to test a skill, benchmark skill performance with variance analysis, or optimize a skill's description for better triggering accuracy.
---

# Skill Creator

A skill for creating new skills and iteratively improving them.

## Core loop

1. Decide what the skill should do
2. Write a draft SKILL.md
3. Run test prompts
4. Evaluate results (qualitatively and quantitatively)
5. Rewrite based on feedback
6. Repeat until satisfied
7. Expand test set and try again at scale

---

## Creating a skill

### Capture Intent

Extract from conversation history:
- Tools used, sequence of steps, corrections made, input/output formats

Answer these questions:
1. What should this skill enable Claude to do?
2. When should this skill trigger?
3. What is the expected output format?
4. Should we set up test cases?

### Write the SKILL.md

Required frontmatter fields:
- **name**: Skill identifier
- **description**: When to trigger + what it does. Make descriptions slightly "pushy" to combat undertriggering.

### Skill anatomy

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter
│   └── Markdown instructions
└── Bundled Resources (optional)
    ├── scripts/    — Executable code
    ├── references/ — Docs loaded into context
    └── assets/     — Files used in output
```

### Progressive disclosure levels

1. Metadata (name + description) — always in context
2. SKILL.md body — in context when skill triggers (keep under 500 lines)
3. Bundled resources — loaded as needed

---

## Updating an existing skill

- Preserve the original name and directory name
- Copy to a writeable location before editing
- Package from the copy

---

## Claude.ai-specific notes

- No subagents: run test cases one at a time
- No browser: present results directly in conversation
- Skip quantitative benchmarking — focus on qualitative feedback
- Description optimisation requires `claude` CLI — skip on Claude.ai
