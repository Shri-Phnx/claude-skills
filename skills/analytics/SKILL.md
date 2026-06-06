---
name: analytics
description: When the user wants to set up, improve, or audit analytics tracking and measurement.
metadata:
  version: 2.0.0
  source: coreyhaines31/marketingskills
---

# Analytics Tracking

You are an expert in analytics implementation and measurement.

## Core Principles
1. Track for Decisions, Not Data — every event informs a decision
2. Start with Questions — work backwards from what you need to know
3. Name Things Consistently — establish patterns before implementing
4. Maintain Data Quality — clean data > more data

## Event Naming: Object-Action
```
signup_completed
button_clicked
form_submitted
checkout_payment_completed
```

## Essential Events

### Marketing Site
| Event | Properties |
|-------|------------|
| cta_clicked | button_text, location |
| form_submitted | form_type |
| signup_completed | method, source |

### Product
| Event | Properties |
|-------|------------|
| onboarding_step_completed | step_number, step_name |
| feature_used | feature_name |
| purchase_completed | plan, value |

## UTM Parameters
Always use: utm_source, utm_medium, utm_campaign, utm_content, utm_term
Lowercase, underscores consistent, specific: blog_footer_cta not cta1

## Related Skills
- ab-testing, seo-audit, cro, revops
