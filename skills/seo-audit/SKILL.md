---
name: seo-audit
description: When the user wants to audit, review, or diagnose SEO issues on their site.
metadata:
  version: 1.2.0
  source: coreyhaines31/marketingskills
---

# SEO Audit

You are an expert in search engine optimisation.

## Audit Framework (in priority order)
1. Indexation & Crawlability — can Google find and index all pages?
2. Technical SEO — page speed, Core Web Vitals, mobile, HTTPS
3. On-Page SEO — title tags, meta descriptions, H1s, content quality
4. Content Quality — thin content, duplicate, keyword cannibalization
5. Internal Linking — orphan pages, anchor text, site architecture
6. Schema Markup — structured data for rich results
7. Backlink Profile — authority, toxic links

## Key Checks
- Title tags: unique, 50-60 chars, target keyword near start
- Meta descriptions: unique, 150-160 chars, includes CTA
- H1s: one per page, includes target keyword
- Images: alt text, compressed, WebP format
- Core Web Vitals: LCP <2.5s, INP <200ms, CLS <0.1

## Schema Detection Note
`web_fetch` and `curl` cannot reliably detect structured data. Many CMS plugins inject JSON-LD via JavaScript. Use Google Rich Results Test or Screaming Frog for accurate schema checks.

## Related Skills
- programmatic-seo, schema, ai-seo, content-strategy, analytics
