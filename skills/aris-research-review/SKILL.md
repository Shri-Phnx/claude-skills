---
name: research-review
description: Get a deep critical review of research from an external reviewer backend (Codex or manual). Use when user says "review my research", "get external review", or wants critical feedback on research ideas, papers, or experimental results.
argument-hint: [topic-or-scope]
allowed-tools: Bash(*), Read, Grep, Glob, Write, Edit, mcp__codex__codex, mcp__codex__codex-reply, mcp__manual_review__review, mcp__manual_review__review_reply
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/research-review
---

# Research Review via External Reviewer Backend

Get a multi-round critical review of research work with maximum reasoning depth.

## Constants
- **REVIEWER_MODEL = `gpt-5.5`**
- **REVIEWER_BACKEND = `codex`** — Override: `— reviewer: manual`

## Prerequisites
```bash
claude mcp add codex -s user -- codex mcp-server
```

## Workflow

### Step 1: Gather Research Context
Read project narrative documents, memory/notes files, prior review documents.
Identify: core claims, methodology, key results, known weaknesses.

### Step 2: Initial Review (Round 1)
```
mcp__codex__codex:
  config: {"model_reasoning_effort": "xhigh"}
  prompt: |
    [Full research context + specific questions]
    Please act as a senior ML reviewer (NeurIPS/ICML level). Identify:
    1. Logical gaps or unjustified claims
    2. Missing experiments that would strengthen the story
    3. Narrative weaknesses
    4. Whether the contribution is sufficient for a top venue
    Be brutally honest.
```

### Step 3: Iterative Dialogue (Rounds 2-N)
Use `mcp__codex__codex-reply` with saved `threadId`.

Key follow-up patterns:
- "What's the minimum experiment to satisfy concern Z?"
- "Design the minimal additional experiment package (highest acceptance lift per GPU week)"
- "Write a mock NeurIPS review with scores"
- "Give me a results-to-claims matrix for possible experimental outcomes"

### Step 4: Document
Save round-by-round summary, claims matrix, prioritized TODO list to a review document.

## Key Rules
- ALWAYS use `config: {"model_reasoning_effort": "xhigh"}`
- Send comprehensive context in Round 1
- Focus on ACTIONABLE feedback
- Do NOT wrap in `/loop`, `/schedule`, or `CronCreate`
