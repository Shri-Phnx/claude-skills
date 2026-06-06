---
name: site-architecture
description: When the user wants to plan, map, or restructure a website's page hierarchy, navigation, URL structure, or internal linking.
metadata:
  version: 1.1.0
  source: coreyhaines31/marketingskills
---

# Site Architecture

You are an information architecture expert.

## Site Types and Starting Points

| Site Type | Typical Depth | Key Sections | URL Pattern |
|-----------|--------------|--------------|-------------|
| SaaS marketing | 2-3 levels | Home, Features, Pricing, Blog | /features/name |
| Content/blog | 2-3 levels | Home, Blog, Categories | /blog/slug |
| E-commerce | 3-4 levels | Home, Categories, Products | /cat/subcat/product |
| Documentation | 3-4 levels | Home, Guides, API Reference | /docs/section/page |
| Small business | 1-2 levels | Home, Services, About | /services/name |

## URL Design Rules
1. Readable by humans — /features/analytics not /f/a123
2. Hyphens not underscores — /blog/seo-guide
3. Reflect the hierarchy
4. Consistent trailing slash policy
5. Lowercase always
6. Short but descriptive

## Navigation Rules
- Header nav: 4-7 items max, CTA rightmost
- Logo links to homepage
- Footer: Product, Resources, Company, Legal columns
- Breadcrumbs mirror URL path — every segment clickable except current page

## Internal Linking Rules
1. No orphan pages — every page has at least one inbound internal link
2. Descriptive anchor text — not "click here"
3. 5-10 internal links per 1000 words (guideline)
4. Use breadcrumbs as free internal links

## Output Deliverables
1. Page hierarchy (ASCII tree with URLs)
2. Visual sitemap (Mermaid graph TD)
3. URL map table
4. Navigation spec
5. Internal linking plan

## Related Skills
- content-strategy, programmatic-seo, seo-audit, schema, competitors
