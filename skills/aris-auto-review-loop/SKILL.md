---
name: auto-review-loop
description: Autonomous multi-round research review loop. Repeatedly reviews via external reviewer backend (Codex or manual), implements fixes, and re-reviews until positive assessment or max rounds reached. Use when user says "auto review loop", "review until it passes", or wants autonomous iterative improvement.
argument-hint: [topic-or-scope]
allowed-tools: Bash(*), Read, Grep, Glob, Write, Edit, Skill, mcp__codex__codex, mcp__codex__codex-reply, mcp__manual_review__review, mcp__manual_review__review_reply
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/auto-review-loop
---

# Auto Review Loop: Autonomous Research Improvement

Autonomously iterate: review → implement fixes → re-review, until positive assessment or MAX_ROUNDS reached.

## Constants
- **MAX_ROUNDS = 4**
- **POSITIVE_THRESHOLD**: score >= 6/10 AND verdict ∈ {"ready", "almost"} — both must hold
- **REVIEW_DOC**: `review-stage/AUTO_REVIEW.md`
- **REVIEWER_MODEL = `gpt-5.5`**
- **REVIEWER_BACKEND = `codex`** — Override: `— reviewer: manual`
- **REVIEWER_DIFFICULTY = medium** — `medium | hard | nightmare`

> Override: `/auto-review-loop "topic" — human checkpoint: true, difficulty: hard`

## Loop (up to MAX_ROUNDS)

### Phase A: Review
```
mcp__codex__codex:
  config: {"model_reasoning_effort": "xhigh"}
  prompt: |
    [Round N/MAX_ROUNDS]
    Please act as a senior ML reviewer (NeurIPS/ICML level).
    1. Score this work 1-10
    2. List critical weaknesses (ranked by severity)
    3. Specify MINIMUM fix for each weakness
    4. State clearly: READY for submission? Yes/No/Almost
```
For round 2+: use `mcp__codex__codex-reply` with saved `threadId`.

Difficulty modes:
- **medium**: standard MCP review
- **hard**: + Reviewer Memory + Debate Protocol
- **nightmare**: + GPT reads repo directly via codex exec

### Phase B: Parse Assessment
Extract: Score, Verdict, Action items.
**STOP if**: score >= 6 AND verdict ∈ {"ready", "almost"}

### Phase C: Implement Fixes
Code changes, run experiments, update analysis, update documentation.

### Phase D: Wait for Results
Monitor remote sessions, collect output files.

### Phase E: Document Round
Append to `review-stage/AUTO_REVIEW.md`. Write `REVIEW_STATE.json`.

## Key Rules
- ALWAYS use `config: {"model_reasoning_effort": "xhigh"}`
- Save `threadId` from first call; use `codex-reply` for rounds 2+
- Save FULL raw response verbatim
- Implement fixes BEFORE re-reviewing
- Do NOT wrap in `/loop`, `/schedule`, or `CronCreate` — already loops internally
