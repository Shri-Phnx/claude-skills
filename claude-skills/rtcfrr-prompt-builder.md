---
name: rtcfrr-prompt-builder
description: >
  Apply the R·T·C·F·R·R Universal Prompt Template to generate structured, high-quality AI prompts
  for any use case. Use this skill whenever the user wants to: build a prompt using the RTCFRR
  framework, fill in the Role/Task/Context/Format/Rules/Review template, craft a prompt for CV
  tailoring, LinkedIn content, recruiter outreach, CareerForge content, job applications, or any
  professional deliverable. Also trigger when the user says "use the RTCFRR template", "build me a
  prompt", "create a prompt for X", "apply the prompt framework", or references any of the six
  sections (Role, Task, Context, Format, Rules, Review). Always use this skill — do not build
  prompts from scratch when this framework is available.
---

# RTCFRR Prompt Builder
### R·T·C·F·R·R — Universal Prompt Template Skill

## What this skill does

Generates a complete, structured AI prompt using the six-section R·T·C·F·R·R framework.

---

## The Six Sections

### R — ROLE
```
ROLE: You are a [job title / expert persona] with [X years] of experience in [domain],
specialising in [focus area] for [market/audience].
```

### T — TASK
```
TASK: [Action verb] + [specific deliverable] for [platform / channel / purpose].
```

### C — CONTEXT
```
CONTEXT:
- Audience:         [who will read/use/act on this]
- Problem:          [pain point or gap being addressed]
- Goal:             [what success looks like]
- My background:    [relevant credential or data point to weave in]
- Additional context: [anything else the AI must know]
```

### F — FORMAT
```
FORMAT:
- Structure:    [sections or flow]
- Length:       [word/line/page limit]
- Language:     [tone] + [British / US English]
- Output type:  [bullets / prose / table / JSON / numbered list / code]
- Extras:       [hashtags / headers / citations — include or exclude]
```

### R — RULES + REFERENCES
```
RULES + REFERENCES:
Do:
- [Non-negotiable to include]
- [Style or tone requirement]

Do not:
- [Hard restriction]
- [Common AI drift to block]

Reference example:
"[Paste a sample line or structure you want matched]"
```

### R — REVIEW + REFINE
```
REVIEW + REFINE:
After generating, audit against these criteria:
- [Criterion 1]
- [Criterion 2]
- [Criterion 3]
Then rewrite the final version only. Do not show the draft.
```

---

## Shrinivas-specific defaults

| Use case | ROLE default | FORMAT defaults |
|---|---|---|
| UAE job application | Senior IT Program Manager, 15+ yrs, ITAM/ITSM/ServiceNow, UAE/GCC market | British English, ATS-clean prose, 400-600 words |
| LinkedIn post | Senior IT leader and AI practitioner, thought leadership for GCC professionals | British English, <150 words, conversational, no em-dashes |
| Recruiter outreach | Job seeker with ITAM/ITSM/ServiceNow expertise targeting UAE market | British English, 300-character limit |
| CareerForge content | Career coach targeting professionals aged 25-40 | British English, BuzzFeed-style hook, Instagram or LinkedIn format |

**Always apply:** British English. Never add SAFe or SOX. Always include Generative AI and Prompt Engineering.

---

## Quick-fill template

```
ROLE: You are a [job title / expert persona] with [X years] of experience in [domain],
specialising in [focus area] for [market/audience].

TASK: [Action verb] + [specific deliverable] for [platform / channel / purpose].

CONTEXT:
- Audience:           [who will read/use/act on this]
- Problem:            [pain point or gap being addressed]
- Goal:               [what success looks like]
- My background:      [relevant credential or data point to weave in]
- Additional context: [anything else the AI must know]

FORMAT:
- Structure:    [sections or flow]
- Length:       [word/line/page limit]
- Language:     [tone] + [British / US English]
- Output type:  [bullets / prose / table / JSON / numbered list / code]
- Extras:       [hashtags / headers / citations]

RULES + REFERENCES:
Do:
- [Non-negotiable to include]

Do not:
- [Hard restriction]

Reference example:
"[Paste a sample line]"

REVIEW + REFINE:
After generating, audit against these criteria:
- [Criterion 1]
- [Criterion 2]
Then rewrite the final version only. Do not show the draft.
```
