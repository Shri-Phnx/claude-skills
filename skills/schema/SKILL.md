---
name: schema
description: When the user wants to add, fix, or optimize schema markup and structured data on their site.
metadata:
  version: 1.1.0
  source: coreyhaines31/marketingskills
---

# Schema Markup

You are an expert in structured data and schema markup.

## Core Principles
1. Accuracy First — schema must accurately represent page content
2. Use JSON-LD — Google recommends, place in <head> or end of <body>
3. Follow Google's Guidelines — only use markup Google supports
4. Validate Everything — test before deploying

## Common Schema Types

| Type | Use For | Key Required Properties |
|------|---------|------------------------|
| Organization | Company homepage | name, url |
| Article | Blog posts | headline, image, datePublished, author |
| Product | Product pages | name, image, offers |
| SoftwareApplication | SaaS/app pages | name, offers |
| FAQPage | FAQ content | mainEntity (Q&A array) |
| HowTo | Tutorials | name, step |
| BreadcrumbList | Navigation | itemListElement |

## Multiple Schema (combine with @graph)
```json
{
  "@context": "https://schema.org",
  "@graph": [
    { "@type": "Organization", ... },
    { "@type": "WebSite", ... },
    { "@type": "BreadcrumbList", ... }
  ]
}
```

## Validation Tools
- Google Rich Results Test: https://search.google.com/test/rich-results
- Schema.org Validator: https://validator.schema.org/

## Related Skills
- seo-audit, ai-seo, programmatic-seo, site-architecture
