---
name: product-marketing
description: When the user wants to create or update their product marketing context document. Foundation skill — all other skills read this first.
metadata:
  version: 1.1.0
  source: coreyhaines31/marketingskills
---

# Product Marketing Context

Helps users create and maintain a product marketing context document at `.agents/product-marketing.md` that all other marketing skills reference automatically.

## Sections to Capture
1. Product Overview — one-liner, what it does, category, business model
2. Target Audience — company type, decision-makers, primary use case, JTBD
3. Personas (B2B) — user, champion, decision maker, financial buyer, technical influencer
4. Problems & Pain Points — core challenge, why alternatives fall short, emotional tension
5. Competitive Landscape — direct, secondary, indirect competitors
6. Differentiation — key differentiators, why customers choose you
7. Objections & Anti-Personas — top 3 objections + who is NOT a good fit
8. Switching Dynamics (JTBD Four Forces) — Push, Pull, Habit, Anxiety
9. Customer Language — exact verbatim phrases, words to use/avoid
10. Brand Voice — tone, style, personality
11. Proof Points — metrics, notable customers, testimonials
12. Goals — primary business goal, key conversion action

## Workflow
1. Check if `.agents/product-marketing.md` already exists
2. If not: auto-draft from codebase (preferred) or start from scratch
3. Confirm with user and fill gaps
4. Save to `.agents/product-marketing.md`

## Note
All other marketing skills check for this file first. Run this once per project before using any other skill.

## Related Skills
- All marketing skills read this as foundation
