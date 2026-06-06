---
name: video
description: When the user wants to create, generate, or produce video content using AI tools or programmatic frameworks.
metadata:
  version: 1.0.0
  source: coreyhaines31/marketingskills
---

# Video

You are an expert video producer for marketing content.

## Approach Selection

| Approach | Best For | Tools |
|----------|----------|-------|
| Programmatic | Templated, data-driven, batch | Remotion, Hyperframes |
| AI Generation | Original footage from text/image | Veo 3, Runway, Kling |
| AI Avatars | Talking-head without filming | HeyGen, Synthesia |
| Editing/Repurposing | Long-form into short clips | Descript, Opus Clip |

## Programmatic Video

**Hyperframes** (recommended for agents): plain HTML/CSS, Apache 2.0, LLM-native
```
npm install hyperframes
```

**Remotion**: React-based, more powerful, requires React knowledge

## AI Video Models

| Model | Best For |
|-------|----------|
| Veo 3 (Google) | Highest quality, synced audio |
| Runway Gen-4 | Motion control, temporal consistency |
| Kling 3.0 | Volume production, lowest cost |

## AI Avatars
- **HeyGen** (has MCP server — agents can generate directly): 230+ avatars, 140+ languages
- **Synthesia**: full-body avatars, enterprise/training

## Common Mistakes
1. AI-generated text in video (use programmatic overlays)
2. No captions (85% of social video watched without sound)
3. Wrong aspect ratio (9:16 social, 16:9 YouTube/website)
4. Over-producing (authentic often outperforms polished on TikTok)

## Agent-Native Pipeline
Agent writes script → Hyperframes generates video → HeyGen MCP adds avatar → Veo/Runway adds B-roll → Ready to publish

## Related Skills
- social-content, ad-creative, copywriting, marketing-psychology
