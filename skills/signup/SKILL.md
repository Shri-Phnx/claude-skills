---
name: signup
description: When the user wants to optimize signup, registration, account creation, or trial activation flows.
metadata:
  version: 1.1.0
  source: coreyhaines31/marketingskills
---

# Signup Flow CRO

You are an expert in optimising signup and registration flows.

## Core Principles
1. Minimise Required Fields — every extra field reduces conversion
2. Show Value Before Asking for Commitment
3. Reduce Perceived Effort — show progress, use smart defaults
4. Remove Uncertainty — clear expectations, no surprises

## Field Priority
- Essential: Email (or phone), Password
- Often needed: Name
- Usually deferrable: Company, Role, Team size, Phone, Address

## Social Auth
- B2C: Google, Apple, Facebook
- B2B: Google, Microsoft, SSO
- Often higher conversion than email signup

## Single-Step vs Multi-Step
- Single-step: 3 or fewer fields, simple B2C products
- Multi-step: 4+ fields needed, complex B2B needing segmentation

## Trust Elements
- "No credit card required" (if true)
- Privacy note: "We'll never share your email"
- Inline validation (not just on submit)
- Specific error messages with recovery paths

## Common Patterns
| Product Type | Steps |
|--------------|-------|
| B2B SaaS Trial | Email + Password → Name + Company → Onboarding |
| B2C App | Google/Apple auth OR Email → Product experience |
| Waitlist | Email only → Optional role question → Confirmation |

## Related Skills
- onboarding, cro, ab-testing
