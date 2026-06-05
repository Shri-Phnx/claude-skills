---
name: kill-argument
description: "Two-thread adversarial review: a fresh reviewer constructs the strongest 200-word rejection memo, then a second fresh reviewer defends the paper point-by-point. Use before submitting a theory paper, for rebuttal preparation, or to find the worst-case rejection paragraph."
argument-hint: [paper-directory]
allowed-tools: Bash(*), Read, Write, Edit, Grep, Glob, mcp__codex__codex
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/kill-argument
---

# Kill Argument Exercise: Adversarial Attack-Defense Review

Stress-test the headline claims of a paper: **$ARGUMENTS**

## Why This Exists
Standard reviews produce balanced weakness lists. Adversarial reviewers must commit:
their job is to convince the area chair to reject in 200 words. This skill forces that
commitment, then defends point-by-point.

## Constants
- **ATTACK_LENGTH** = approximately 200 words
- **CONTEXT_POLICY** = `fresh` — each thread is a fresh `mcp__codex__codex` call, NEVER `codex-reply`
- **CLASSIFICATION**: `answered_by_current_text` / `partially_answered` / `still_unresolved`
- **OUTPUT**: `KILL_ARGUMENT.md` + `KILL_ARGUMENT.json` in the paper directory

## Workflow

### Step 1: Discover paper files
Locate `main.tex` and all source files.

### Step 2: Attack memo (Thread 1 — fresh codex)
```
mcp__codex__codex:
  model: gpt-5.5
  config: {"model_reasoning_effort": "xhigh"}
  sandbox: read-only
  prompt: |
    You are simulating a hostile NeurIPS/ICLR/ICML reviewer.
    Construct the SINGLE STRONGEST argument for rejecting this paper in ~200 words.
    Focus on: theorem validity, assumption-vs-claim mismatch, missing proof obligations,
    limit-order ambiguity, claim-vs-evidence gap, scope overclaim.
    Single argument, not a list. Cite specific file:line locations. Do NOT hedge.
```

### Step 3: Adjudication memo (Thread 2 — fresh codex)
```
mcp__codex__codex:
  model: gpt-5.5
  config: {"model_reasoning_effort": "xhigh"}
  sandbox: read-only
  prompt: |
    You are an independent adjudicator. Decompose the attack into 3-7 atomic points.
    For each, classify: answered_by_current_text / partially_answered / still_unresolved
    Cite file:line evidence. State severity if unresolved. Give recommended fix.
    Then: counts summary, net assessment, top 3 action items.
```

### Step 4: Write KILL_ARGUMENT.md and KILL_ARGUMENT.json
Verdict mapping:
| Verdict | Trigger |
|---|---|
| `FAIL` | ≥1 `still_unresolved` at `critical` severity |
| `WARN` | ≥1 `still_unresolved` at `major/minor` |
| `PASS` | 0 `still_unresolved`, only minor `partially_answered` |
| `NOT_APPLICABLE` | <2 theorem environments AND no scope claims |

## Key Rules
- **Fresh thread per call** — NEVER use `codex-reply`
- **Zero prior context** — neither thread receives prior round reviews
- **Attack must commit** — single ~200-word argument, no hedging
- **Verdict is computed by the skill** — never let adjudicator self-grade the top-level verdict
- **Detect-only** — does not auto-modify paper files

## When NOT to Use
- Empirical papers without theorems → use `/research-review` instead
- Very early drafts (headline not stable)
- Papers with ongoing experiments