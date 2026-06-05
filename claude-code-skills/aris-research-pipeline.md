---
name: research-pipeline
description: "Full end-to-end research pipeline: idea discovery → experiments → review → paper. Use when user says 'full pipeline', 'end-to-end research', or wants the complete autonomous ML research lifecycle."
argument-hint: "[research-direction] [— resume <run_id>]"
allowed-tools: Bash(*), Read, Write, Edit, Grep, Glob, WebSearch, WebFetch, Skill, mcp__codex__codex, mcp__codex__codex-reply
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/research-pipeline
---

# Full Research Pipeline: Idea → Experiments → Submission

End-to-end autonomous research workflow for: **$ARGUMENTS**

## Pipeline
```
/idea-discovery → /experiment-bridge → /auto-review-loop → /paper-writing (optional)
├── Workflow 1 ──┤├── Workflow 1.5 ──┤├── Workflow 2 ───┤ ├── Workflow 3 ──┤
```

## Constants
- **AUTO_PROCEED = true**
- **AUTO_WRITE = false** — set `true` to auto-invoke `/paper-writing` after Stage 4
- **VENUE = ICLR** — used when `AUTO_WRITE=true`
- **RESUMABLE = true** — records per-stage state for crash recovery via `— resume <run_id>`

> Override: `/research-pipeline "topic" — AUTO_PROCEED: false, auto_write: true, venue: NeurIPS`

## Stages

### Stage 1: Idea Discovery
`/idea-discovery "$ARGUMENTS"` → `idea-stage/IDEA_REPORT.md`
**Gate 1:** Present top ideas. AUTO_PROCEED=true auto-selects #1 after 10 seconds.

### Stage 2: Experiment Bridge
`/experiment-bridge "$CHOSEN_IDEA_TITLE"`
1. Parse `refine-logs/EXPERIMENT_PLAN.md`
2. Implement experiment code
3. Cross-model code review by GPT-5.5 xhigh
4. Sanity check on smallest experiment
5. Deploy (≤5 jobs → `/run-experiment`, ≥10 → `/experiment-queue`)
6. Collect initial results

### Stage 3: Auto Review Loop
`/auto-review-loop "$ARGUMENTS — [chosen idea]"` — up to 4 rounds

### Stage 4: Research Summary
Generate `NARRATIVE_REPORT.md` from all accumulated materials.

### Stage 5: Paper Writing (Optional)
`/paper-writing "NARRATIVE_REPORT.md" — venue: $VENUE` — only when `AUTO_WRITE=true`

## Typical Timeline
| Stage | Duration | Can sleep? |
|-------|----------|------------|
| 1. Idea Discovery | 30-60 min | Yes ✅ |
| 2. Experiment Bridge | 30-120 min | Yes ✅ |
| 3. Auto Review | 1-4 hours | Yes ✅ |

**Sweet spot**: Run Stage 1 in the evening, launch Stages 2-3 before bed, wake up to a reviewed paper.