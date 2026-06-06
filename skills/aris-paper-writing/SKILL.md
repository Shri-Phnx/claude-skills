---
name: paper-writing
description: "Workflow 3: Full paper writing pipeline from a narrative report to a polished, submission-ready PDF. Chains: /paper-plan → /paper-figure → /paper-write → /paper-compile → /auto-paper-improvement-loop."
argument-hint: "[narrative-report-path-or-topic] [— style-ref: <source>]"
allowed-tools: Bash(*), Read, Write, Edit, Grep, Glob, Skill, mcp__codex__codex, mcp__codex__codex-reply
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/paper-writing
---

# Workflow 3: Paper Writing Pipeline

Orchestrate a complete paper writing workflow for: **$ARGUMENTS**

## Pipeline
```
/paper-plan → /paper-figure → /paper-write → /paper-compile → /auto-paper-improvement-loop
  (outline)     (plots)        (LaTeX)        (build PDF)       (review & polish ×2)
```

## Constants
- **VENUE = `ICLR`** — Options: `ICLR`, `NeurIPS`, `ICML`, `CVPR`, `ACL`, `AAAI`, `ACM`, `IEEE_JOURNAL`, `IEEE_CONF`
- **MAX_IMPROVEMENT_ROUNDS = 2**
- **REVIEWER_MODEL = `gpt-5.5`**
- **ILLUSTRATION = `figurespec`** — Options: `figurespec`, `gemini`, `mermaid`, `codex-image2`, `false`

> Override: `/paper-writing "NARRATIVE_REPORT.md" — venue: NeurIPS, illustration: gemini`

## Phases

### Phase 0: Assurance Setup
Resolve `assurance` level: `draft` (default) or `submission`.

### Phase 1: Paper Plan
`/paper-plan "$ARGUMENTS"` → `PAPER_PLAN.md`
- Parse NARRATIVE_REPORT.md for claims, evidence, figure descriptions
- Build Claims-Evidence Matrix
- Design 5-8 section structure
- GPT-5.5 reviews the plan

### Phase 2: Figure Generation
`/paper-figure "PAPER_PLAN.md"` — data plots and LaTeX comparison tables

#### Phase 2b: Architecture & Illustration
- **figurespec**: deterministic JSON → SVG, best for architecture/workflow diagrams
- **gemini**: AI-generated, best for qualitative method illustrations
- **mermaid**: flowcharts, free, no API key
- **false**: skip, all figures manual

### Phase 3: LaTeX Writing
`/paper-write "PAPER_PLAN.md"` → `paper/main.tex`, `sections/*.tex`, `references.bib`

### Phase 4: Compilation
`/paper-compile "paper/"` — `latexmk -pdf` with auto-fix and multi-pass

### Phase 4.5–4.7: Proof & Claim Audits (if applicable)
- `/proof-checker` — for theory papers with theorems/lemmas
- `/paper-claim-audit` — verify numbers match raw results

### Phase 5: Auto Improvement Loop
`/auto-paper-improvement-loop "paper/"` — 2 rounds: review → fix → recompile

### Phase 5.5–5.8: Final Audits (submission mode)
- `/paper-claim-audit`, `/kill-argument`, `/citation-audit`

## Output
- `paper/main_round0_original.pdf`, `paper/main_round1.pdf`, `paper/main_round2.pdf`
- `paper/PAPER_IMPROVEMENT_LOG.md`

**Total: ~45-90 min** from narrative report to polished PDF.
