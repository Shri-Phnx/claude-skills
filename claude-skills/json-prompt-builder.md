---
name: json-prompt-builder
description: >
  Apply JSON prompting best practices to generate structured, machine-parseable, deterministic
  AI outputs using the R·T·C·S·F·R·E·V framework. Use this skill whenever the user wants to:
  build a JSON prompt or schema-driven prompt, design prompts for n8n/Make.com/LangChain/Python
  pipelines, extract structured data via LLMs, build scoring/evaluation/classification/routing
  prompts, get reliable consistent AI outputs for automation workflows, or create agent node
  prompts. Trigger when the user says "JSON prompt", "structured output", "schema", "parse AI
  output", "extract fields", "AI workflow node", "n8n prompt", "deterministic output", or asks
  how to get reliable/consistent/machine-readable outputs from AI. ALWAYS use this skill when
  the deliverable is a prompt whose output will be consumed by code, a parser, or another
  workflow node — not a human reader.
---

# JSON Prompt Builder Skill
### R·T·C·S·F·R·E·V Framework for Structured AI Output

---

## What This Skill Does

Generates complete, production-grade JSON prompts using the R·T·C·S·F·R·E·V framework —
an extension of the RTCFRR template purpose-built for machine-consumed AI outputs.

Use this when the AI's output feeds code, a parser, an automation node, or another AI agent —
not when it feeds a human reader.

---

## The R·T·C·S·F·R·E·V Framework

| Layer | Section | Purpose | Where |
|---|---|---|---|
| R | Role | Who the AI is | System prompt |
| T | Task | What to do with this input | User prompt |
| C | Context | The actual input payload | User prompt |
| S | Schema | Exact JSON output shape | System prompt |
| F | Format | Output constraints | System prompt |
| R | Rules | Hard guardrails | System prompt |
| E | Examples | Few-shot input/output pairs | User prompt (first call) |
| V | Validation | Pre-response self-audit | System prompt |

**Key architectural rule:** Schema, Format, Rules, and Validation go in the **system prompt** (static).
Task, Context, and Examples go in the **user prompt** (dynamic, changes per call).

---

## Step 0 — Gap Check (MANDATORY before building anything)

Before writing a single line of schema or prompt, run a gap check against the 8 required inputs.

### The 8 Required Inputs

| # | Input | Why It Matters | Ask If Missing |
|---|---|---|---|
| 1 | **Use case / variant** | Determines schema shape and field types | "Is this for extraction, scoring, classification, content generation, or an agent node?" |
| 2 | **Input type** | Defines what data the prompt receives | "What will be passed in?" |
| 3 | **Output fields** | The actual schema keys needed | "What specific fields do you need in the output?" |
| 4 | **Enum values** | Required for verdict / category / routing fields | "What are the allowed values for [verdict/category/status]?" |
| 5 | **Null behaviour preference** | Prevents parser crashes on missing data | "If a field has no data, should it return null, an empty string, or be omitted?" |
| 6 | **Target platform** | Affects seeding, temperature, and format rules | "Where will this run?" |
| 7 | **Example availability** | Few-shot examples dramatically improve accuracy | "Do you have a sample input and ideal output?" |
| 8 | **Scoring criteria** *(Variant B only)* | Required to weight the scoring correctly | "What criteria should be scored, and how?" |

---

## Schema Variants

### Variant A — Data Extraction
```json
{
  "extracted_fields": { "name": "string", "years_experience": "integer", "skills": "array of strings" },
  "extraction_confidence": "integer — 0 to 100",
  "unextractable_fields": "array of strings"
}
```

### Variant B — Scoring & Fit Assessment
```json
{
  "reasoning": "string",
  "overall_score": "integer — 0 to 100",
  "matched_signals": "array of strings",
  "gap_signals": "array of strings",
  "recommendation": "enum — proceed | proceed_with_caveat | hold | reject",
  "confidence": "integer — 0 to 100"
}
```

### Variant C — Classification & Routing
```json
{
  "reasoning": "string",
  "primary_category": "enum — [categories]",
  "routing_action": "enum — [actions]",
  "confidence": "integer — 0 to 100",
  "flags": "array of strings"
}
```

### Variant D — Structured Content Generation
```json
{
  "hook": "string — max 15 words",
  "body": "string — max 150 words",
  "cta": "string — max 20 words",
  "hashtags": "array of strings — max 5",
  "tone_check": "enum — human | borderline | ai-sounding",
  "word_count": "integer"
}
```

### Variant E — Agent Node
```json
{
  "node_id": "string",
  "status": "enum — success | partial | failed",
  "reasoning": "string",
  "output_data": "object",
  "next_action": "enum — continue | branch_a | branch_b | escalate | stop"
}
```

---

## Shrinivas-Specific Defaults

| Workflow | Variant | Notes |
|---|---|---|
| CareerForge lead scoring | B — Scoring | Add `tier` field: silver/gold/diamond |
| CV fit assessment | B — Scoring | Include `ats_keywords_matched` array |
| LinkedIn content generation | D — Content | British English in rules; no em-dashes |
| n8n automation node | E — Agent Node | Always include `next_action` routing field |
| Job description extraction | A — Extraction | Add `salary_range`, `remote_eligible` fields |
| Recruiter profile analysis | A — Extraction | Add `agency`, `specialisation`, `market` fields |
