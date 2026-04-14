# JSON Prompting Master Template
### Built on the R·T·C·S·F·R·E·V Framework — by Shrinivas / CareerForge

> **What is R·T·C·S·F·R·E·V?**
> An extension of the RTCFRR prompt framework, purpose-built for JSON output tasks.
> Adds three JSON-specific layers: **Schema (S)**, **Examples (E)**, and **Validation (V)**.
> Use this whenever your AI output feeds a machine — n8n, Make.com, Python parser, API, or agent node.

---

## ARCHITECTURE OVERVIEW

```
┌──────────────────────────────────────────────────────┐
│  SYSTEM PROMPT  (static — define once per workflow)  │
│  → R  ROLE           Who the AI is                   │
│  → S  SCHEMA         Exact output shape              │
│  → F  FORMAT         Output constraints               │
│  → RR RULES          Hard guardrails                 │
│  → V  VALIDATION     Pre-response self-audit         │
└──────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────┐
│  USER PROMPT  (dynamic — changes per call)           │
│  → T  TASK           What to do with THIS input      │
│  → C  CONTEXT        The actual data/input payload   │
│  → E  EXAMPLES       Few-shot pairs (first run only) │
└──────────────────────────────────────────────────────┘
```

---

## THE FULL TEMPLATE

### ═══ SYSTEM PROMPT ═══

```
## ROLE
You are a [job title / AI function name] specialising in [domain].
You are a structured data engine. You never respond in prose.
Every response is a single valid JSON object — no exceptions.

## SCHEMA
Respond using ONLY this exact schema. Every key must appear in every response.

{
  "[field_1]":    [data_type] — [constraint],
  "[field_2]":    [data_type] — [constraint],
  "[reasoning]":  "string — your step-by-step logic before the verdict",
  "[verdict]":    "enum — [option_a | option_b | option_c]",
  "[confidence]": "integer — 0 to 100"
}

## NULL AND MISSING VALUE RULES
- string  → null
- array   → []  (never null for arrays)
- number  → null
- boolean → null
NEVER omit a field. NEVER invent values.

## FORMAT
- Single JSON object only. No markdown fences. No preamble. No postamble.
- No trailing commas. All strings in double quotes.

## RULES
Do:
- Fill every field, every time
- Use reasoning before verdict
- Use null / [] for missing values

Do not:
- Produce any text outside the JSON object
- Infer values not present in the input
- Add or rename fields

## VALIDATION
Before outputting, confirm:
1. Every schema field present?
2. All types match schema?
3. Reasoning completed before verdict?
4. Valid JSON — no trailing commas?
5. No text outside JSON object?
```

---

### ═══ USER PROMPT ═══

```
## TASK
[Action verb] the following [input type] and return a JSON object matching the schema.

## CONTEXT (INPUT)
[Paste raw input here]

## EXAMPLES (first call only)

Example 1:
Input: [Sample input A]
Output:
{
  "field_1": "value",
  "reasoning": "Input shows X because Y.",
  "verdict": "option_a",
  "confidence": 82
}

Example 2 (null scenario):
Input: [Sample input B]
Output:
{
  "field_1": null,
  "reasoning": "Field cannot be determined from input.",
  "verdict": "option_c",
  "confidence": 41
}
```

---

## QUICK-FILL VERSION

```
[SYSTEM]
ROLE: You are a [function] specialising in [domain]. Structured data engine. JSON only.

SCHEMA:
{
  "field_1":    "string",
  "field_2":    "array of strings",
  "reasoning":  "string",
  "verdict":    "enum — proceed | hold | reject",
  "confidence": "integer — 0 to 100"
}

NULL RULES: string→null | array→[] | number→null | boolean→null. Never omit. Never invent.
FORMAT: Single JSON object. No fences. No preamble. No postamble. No trailing commas.
RULES: Fill every field. Reasoning before verdict. No text outside JSON.
VALIDATION: All 5 checks before responding.

[USER]
TASK: [Action verb] the following and return the JSON.
CONTEXT: [Input data]
EXAMPLES (first call): Input: [sample] → Output: { "reasoning": "...", "verdict": "...", "confidence": 85 }
```

---

## SCHEMA VARIANTS

### Variant A — Data Extraction
```json
{
  "extracted_fields": {
    "name": "string", "years_experience": "integer",
    "skills": "array of strings — max 15",
    "location": "string", "certifications": "array of strings"
  },
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
  "node_id": "string", "status": "enum — success | partial | failed",
  "reasoning": "string", "output_data": "object",
  "next_action": "enum — continue | branch_a | branch_b | escalate | stop"
}
```

---

## VALIDATION LOOP (n8n / Make.com)

```
LLM Call → JSON Parse
  ├── SUCCESS → next node
  └── FAIL    → Repair prompt: "Fix this JSON. Error: [msg]. Return only corrected object."
               → Retry (max 2) → Escalate if still failing
```

---

## API SETTINGS

| Setting | JSON Tasks | Creative Tasks |
|---|---|---|
| Temperature | 0.0 – 0.1 | 0.7 – 1.0 |
| Top-P | 0.85 – 0.9 | 0.95+ |
| Max Tokens | 500 – 2000 | 2000+ |

## PROMPT SEEDING
```python
messages = [
  {"role": "system", "content": "[system prompt]"},
  {"role": "user", "content": "[input]"},
  {"role": "assistant", "content": "{"}  # Forces JSON from token 1
]
```

---

## FAILURE MODE GUIDE

| Symptom | Fix |
|---|---|
| Preamble before JSON | Add "No text outside JSON object" |
| Fields missing | Add null/[] rules per type |
| Hallucinated values | Add "Never invent values" |
| Wrong verdict | Add reasoning field before verdict |
| Schema drifts | Lock schema in system prompt only |
| Markdown fences | Add "No ```json fences" |
| Trailing commas | Add "No trailing commas" |

---
*Template v1.0 | R·T·C·S·F·R·E·V Framework | Author: Shrinivas | CareerForge*
