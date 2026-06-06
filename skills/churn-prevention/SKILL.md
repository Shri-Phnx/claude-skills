---
name: churn-prevention
description: When the user wants to reduce churn, build cancellation flows, set up save offers, recover failed payments, or implement retention strategies.
metadata:
  version: 2.0.0
  source: coreyhaines31/marketingskills
---

# Churn Prevention

You are an expert in SaaS retention and churn prevention.

## Two Types of Churn

| Type | Cause | Solution |
|------|-------|----------|
| Voluntary | Customer chooses to cancel | Cancel flows, save offers, exit surveys |
| Involuntary | Payment fails | Dunning emails, smart retries, card updaters |

## Cancel Flow Structure
```
Trigger → Survey → Dynamic Offer → Confirmation → Post-Cancel
```

## Dynamic Save Offers

| Cancel Reason | Primary Offer | Fallback |
|---------------|---------------|----------|
| Too expensive | Discount (20-30% for 2-3 months) | Downgrade |
| Not using enough | Pause (1-3 months) | Onboarding session |
| Missing feature | Roadmap preview | Workaround guide |
| Switching competitor | Comparison + discount | Feedback session |
| Temporary need | Pause | Downgrade |

## Dunning Sequence

| Email | Timing | Tone |
|-------|--------|------|
| 1 | Day 0 | Friendly alert |
| 2 | Day 3 | Helpful reminder |
| 3 | Day 7 | Urgency |
| 4 | Day 10 | Final warning |

## Key Metrics
- Cancel flow save rate target: 25-35%
- Pause reactivation rate target: 60-80%
- Dunning recovery rate target: 50-60%

## Related Skills
- emails, paywalls, pricing, onboarding, analytics, ab-testing
