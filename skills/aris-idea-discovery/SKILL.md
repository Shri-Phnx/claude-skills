---
name: idea-discovery
description: "Workflow 1: Full idea discovery pipeline from a broad research direction to validated, pilot-tested ideas. Chains: /research-lit → /idea-creator → /novelty-check → /research-review → /research-refine-pipeline."
argument-hint: [research-direction]
allowed-tools: Bash(*), Read, Write, Edit, Grep, Glob, WebSearch, WebFetch, Skill, mcp__codex__codex, mcp__codex__codex-reply
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/idea-discovery
---

# Workflow 1: Idea Discovery Pipeline

Orchestrate a complete idea discovery workflow for: **$ARGUMENTS**

## Pipeline
```
/research-lit → /idea-creator → /novelty-check → /research-review → /research-refine-pipeline
  (survey)      (brainstorm)    (verify novel)    (critical feedback)  (refine + plan experiments)
```

## Constants
- **OUTPUT_DIR = `idea-stage/`**
- **AUTO_PROCEED = true**
- **REVIEWER_MODEL = `gpt-5.5`**
- **MAX_PILOT_IDEAS = 3**
- **RENDER_HTML = true**

## Phases

### Phase 0: Load Research Brief
Check for `RESEARCH_BRIEF.md` in project root. Use as primary context if found.

### Phase 1: Literature Survey
```
/research-lit "$ARGUMENTS" — sources: all, gemini — composed: idea-stage/IDEA_REPORT.md
```
Build landscape map: sub-directions, approaches, open problems, gaps.
**Checkpoint:** Present landscape summary.

### Phase 2: Idea Generation + Filtering + Pilots
```
/idea-creator "$ARGUMENTS" — composed: idea-stage/IDEA_REPORT.md
```
- Brainstorm 8-12 concrete ideas via GPT-5.5 xhigh
- Filter by feasibility and compute cost
- Run parallel pilot experiments on top 2-3 ideas
- Rank by empirical signal
- Output: `idea-stage/IDEA_REPORT.md`
**Checkpoint:** Present ranked ideas.

### Phase 3: Deep Novelty Verification
```
/novelty-check "[top idea description]"
```

### Phase 4: External Critical Review
```
/research-review "[top idea + pilot results]" — composed: idea-stage/IDEA_REPORT.md
```

### Phase 4.5: Method Refinement + Experiment Planning
```
/research-refine-pipeline "[top idea + feedback]"
```
Output: `refine-logs/FINAL_PROPOSAL.md`, `refine-logs/EXPERIMENT_PLAN.md`

### Phase 5: Final Report
Finalize `idea-stage/IDEA_REPORT.md` with all accumulated information.

## Key Rules
- Don't skip phases — each filters and validates
- Kill ideas early — better to kill 10 bad ideas than implement one and fail
- Empirical signal > theoretical appeal
- ONE canonical doc: `idea-stage/IDEA_REPORT.md` (no scattered files)
