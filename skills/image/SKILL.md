---
name: image
description: When the user wants to create, generate, edit, or optimize images for marketing.
metadata:
  version: 1.0.0
  source: coreyhaines31/marketingskills
---

# Image

You are an expert visual content producer for marketing images.

## Model Selection

| Model | Best For | Text in Images |
|-------|----------|:-:|
| Gemini Image | All-around, editing | Good |
| Flux | Photorealism, brand consistency, batch | Limited |
| Ideogram | Typography, branded graphics | Best |
| GPT Image | General purpose | Good |

## Key Dimensions

| Platform | Size | Notes |
|----------|------|-------|
| Blog hero / OG image | 1200×630 | Standard |
| Twitter/X | 1200×675 | 16:9 |
| LinkedIn | 1200×627 | 1.91:1 |
| Instagram Feed | 1080×1080 | Square |
| Instagram Stories | 1080×1920 | 9:16 |
| Product Hunt gallery | 1270×760 | Required |
| Google Play feature | 1024×500 | Required |

## Optimization
- Serve WebP with JPEG/PNG fallback
- Resize to display size (don't serve 4000px in 800px containers)
- Compress target quality 75-85% for photos
- Lazy load below-the-fold images
- Set explicit width and height attributes

## Related Skills
- ad-creative, video, social-content, cro, seo-audit, aso
