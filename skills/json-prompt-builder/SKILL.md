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

## How to Use This Skill

---

### Step 0 — Gap Check (MANDATORY before building anything)

Before writing a single line of schema or prompt, run a gap check against the 8 required inputs.
**Never skip this step.** A JSON prompt built on incomplete inputs produces unreliable outputs.

#### The 8 Required Inputs

| # | Input | Why It Matters | Ask If Missing |
|---|---|---|---|
| 1 | **Use case / variant** | Determines schema shape and field types | "Is this for extraction, scoring, classification, content generation, or an agent node?" |
| 2 | **Input type** | Defines what data the prompt receives | "What will be passed in — CV text, a job description, a lead form, an email, JSON from another node?" |
| 3 | **Output fields** | The actual schema keys needed | "What specific fields do you need in the output?" |
| 4 | **Enum values** | Required for verdict / category / routing fields | "What are the allowed values for [verdict/category/status]? E.g. proceed / hold / reject" |
| 5 | **Null behaviour preference** | Prevents parser crashes on missing data | "If a field has no data in the input, should it return null, an empty string, or be omitted?" |
| 6 | **Target platform** | Affects seeding, temperature, and format rules | "Where will this run — n8n, Make.com, Python, direct API, or Claude chat?" |
| 7 | **Example availability** | Few-shot examples dramatically improve accuracy | "Do you have a sample input and ideal output I can use as an example?" |
| 8 | **Scoring criteria** *(Variant B only)* | Required to weight the scoring correctly | "What criteria should be scored, and how should each be weighted?" |

#### Gap Check Rules

- If **1–3 inputs are missing**: ask all missing questions in a single grouped message before proceeding.
- If **4+ inputs are missing**: ask the most critical 3 first (use case, input type, output fields), then continue gathering after the user responds.
- If the user says *"just build it"* or *"use defaults"*: proceed with sensible defaults and **clearly state every assumption made** at the top of the output.
- Never silently assume. Every assumption must be visible to the user.

#### Gap Check Message Template

Use this format when asking clarifying questions:

```
Before I build the JSON prompt, I need a few details to make it production-ready:

1. [Question for missing input X]
2. [Question for missing input Y]
3. [Question for missing input Z]

If you'd like me to proceed with defaults for any of these, just say so and I'll flag my assumptions clearly.
```

---

### Step 1 — Identify the Use Case

Determine which variant the user needs:

| Use Case | Variant |
|---|---|
| Extract fields from text | A — Data Extraction |
| Score, rank, or assess | B — Scoring & Fit Assessment |
| Classify and route | C — Classification & Routing |
| Generate structured content | D — Content Generation |
| Feed an agent pipeline node | E — Multi-Step Agent Node |

If still unclear after Step 0, ask: *"Is this for extraction, scoring, classification, content generation, or an agent pipeline node?"*

---

### Step 2 — Build the Schema First

Before writing any prose instructions, define the output schema. Always include:

```json
{
  "field_name": "data_type — constraint",
  "reasoning":  "string — think before deciding (required for eval/scoring tasks)",
  "verdict":    "enum — option_a | option_b | option_c",
  "confidence": "integer — 0 to 100"
}
```

**Schema rules:**
- Every field must have a data type AND a constraint (range, enum values, max length)
- Add `reasoning` before any verdict/decision field — forces chain-of-thought
- Add `confidence` to scoring/classification tasks
- Keep schemas flat — max 2 levels of nesting
- Never use vague types like `"any"` or `"object"` without sub-schema

---

### Step 3 — Assemble the System Prompt

Use this structure exactly:

```
ROLE: You are a [function name] specialising in [domain].
You are a structured data engine. Every response is a single valid JSON object.

SCHEMA:
{ ... exact schema ... }

NULL RULES:
- string → null | array → [] | number → null | boolean → null
- Never omit a field. Never invent values.

FORMAT:
- Single JSON object only. No markdown fences. No preamble. No postamble.
- No trailing commas. All strings in double quotes.

RULES:
Do: Fill every field. Use reasoning before verdict. Use null for unknowns.
Do not: Output text outside the JSON. Add or rename fields.

VALIDATION — run before responding:
1. Every schema field present?
2. All types correct?
3. Reasoning completed before verdict?
4. Valid JSON — no trailing commas?
5. No text outside the JSON object?
```

---

### Step 4 — Assemble the User Prompt

```
TASK: [Action verb] the following [input type] and return a JSON object.

CONTEXT:
[Raw input data — paste as-is, no pre-processing needed]

EXAMPLES (include on first call only):
Input: [sample input]
Output: { "reasoning": "...", "verdict": "...", "confidence": 85 }
```

---

### Step 5 — Apply API Settings

| Setting | Value for JSON Tasks |
|---|---|
| Temperature | 0.0 – 0.1 |
| Top-P | 0.85 – 0.9 |
| Max Tokens | Cap at 500–2000 |
| Prompt Seeding | Add `{"role": "assistant", "content": "{"}` to force JSON from token 1 |

---

### Step 6 — Add a Validation Loop (for workflows)

```
Call → Parse → IF success: continue | IF fail: repair prompt → retry (max 2) → escalate
```

Repair prompt template:
```
Your previous output failed JSON validation.
Error: [parser_error_message]
Return ONLY the corrected JSON object. Do not explain.
Original input: [original_input]
```

---

## Schema Variants (Ready to Use)

### Variant A — Data Extraction
```json
{
  "extracted_fields": {
    "name":            "string",
    "years_experience":"integer",
    "skills":          "array of strings — max 15",
    "location":        "string",
    "certifications":  "array of strings"
  },
  "extraction_confidence": "integer — 0 to 100",
  "unextractable_fields":  "array of strings"
}
```

### Variant B — Scoring & Fit Assessment
```json
{
  "reasoning":       "string",
  "overall_score":   "integer — 0 to 100",
  "matched_signals": "array of strings",
  "gap_signals":     "array of strings",
  "recommendation":  "enum — proceed | proceed_with_caveat | hold | reject",
  "confidence":      "integer — 0 to 100"
}
```

### Variant C — Classification & Routing
```json
{
  "reasoning":        "string",
  "primary_category": "enum — [categories]",
  "routing_action":   "enum — [actions]",
  "confidence":       "integer — 0 to 100",
  "flags":            "array of strings"
}
```

### Variant D — Structured Content Generation
```json
{
  "hook":       "string — max 15 words",
  "body":       "string — max 150 words",
  "cta":        "string — max 20 words",
  "hashtags":   "array of strings — max 5",
  "tone_check": "enum — human | borderline | ai-sounding",
  "word_count": "integer"
}
```

### Variant E — Agent Node
```json
{
  "node_id":     "string — echo input node_id",
  "status":      "enum — success | partial | failed",
  "reasoning":   "string",
  "output_data": "object",
  "next_action": "enum — continue | branch_a | branch_b | escalate | stop"
}
```

---

## Shrinivas-Specific Defaults

Apply these when building JSON prompts for Shrinivas's workflows:

| Workflow | Variant | Notes |
|---|---|---|
| CareerForge lead scoring | B — Scoring | Add `tier` field: silver/gold/diamond |
| CV fit assessment | B — Scoring | Include `ats_keywords_matched` array |
| LinkedIn content generation | D — Content | British English in rules; no em-dashes |
| n8n automation node | E — Agent Node | Always include `next_action` routing field |
| Job description extraction | A — Extraction | Add `salary_range`, `remote_eligible` fields |
| Recruiter profile analysis | A — Extraction | Add `agency`, `specialisation`, `market` fields |

---

## Failure Modes & Fixes

| Symptom | Fix |
|---|---|
| Text before JSON | Add "No text outside the JSON object" to rules |
| Fields missing | Add null/[] enforcement per data type |
| Hallucinated values | Add "Never invent values" to rules |
| Wrong verdict despite good input | Add `reasoning` field before verdict |
| Inconsistent schema shape | Lock schema in system prompt only |
| Markdown fences in output | Add "No ```json fences" to format rules |
| Trailing commas break parser | Add "No trailing commas" to format rules |

---

## Output Delivery

When building a JSON prompt using this skill, always deliver in this order:

1. **Assumptions Block** *(only if any defaults were applied)*
   ```
   ASSUMPTIONS APPLIED:
   - Input type assumed: [CV text / JD text / etc.]
   - Enum values assumed: [proceed | hold | reject]
   - Platform assumed: [n8n]
   - Temperature assumed: 0.0
   Confirm these are correct or tell me what to change.
   ```
2. **System Prompt block** — complete, ready to paste
3. **User Prompt block** — with placeholders clearly marked
4. **Example call** — one filled input/output pair
5. **API settings note** — temperature, max_tokens, seeding tip
6. **Validation loop note** — error handling pattern for the workflow
7. **Offer to refine** — end with: *"Would you like to adjust any field, add an enum value, or test this against a sample input?"*

---

## Reference Template

Full template available at: `json-prompting-template.md`
Covers all 5 variants, quick-fill version, validation loop, API settings, and failure mode guide.
