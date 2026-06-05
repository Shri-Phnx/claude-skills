---
name: frontend-slides
description: Create stunning, animation-rich HTML presentations from scratch or by converting PowerPoint files. Use when the user wants to build a presentation, convert a PPT/PPTX to web, or create slides for a talk/pitch. Helps non-designers discover their aesthetic through visual exploration rather than abstract choices.
source: https://github.com/zarazhangrui/frontend-slides
---

# Frontend Slides

Create zero-dependency, animation-rich HTML presentations that run entirely in the browser.

## Core Principles

1. **Zero Dependencies** — Single HTML files with inline CSS/JS. No npm, no build tools.
2. **Show, Don’t Tell** — Generate visual previews, not abstract choices.
3. **Distinctive Design** — No generic "AI slop." Every presentation must feel custom-crafted.
4. **Fixed 16:9 Stage (NON-NEGOTIABLE)** — Every deck uses a 1920×1080 slide canvas. Never reflow content per device.

## Fixed Stage Rules

- Every slide is authored inside a fixed 1920×1080 stage.
- The stage scales uniformly to fit the viewport (letterbox/pillarbox; no content reflow).
- Slide visibility controlled by `.active` / `.visible` using `visibility`, `opacity`, `pointer-events` — **not** `display: none/block`.
- Use `calc(-1 * clamp(...))` to negate CSS function values. Never `-clamp()` directly.
- Include FULL contents of `viewport-base.css` in every presentation.

## Design Aesthetics

Avoid "AI slop": no Inter/Roboto/Arial fonts, no purple gradients on white, no predictable layouts.

- **Typography:** Fontshare or Google Fonts only. Distinctive choices that elevate aesthetics.
- **Color:** Commit to a cohesive aesthetic. Dominant colors with sharp accents.
- **Motion:** CSS-only preferred. One well-orchestrated page load with staggered reveals > scattered micro-interactions.
- **Backgrounds:** Layer CSS gradients, geometric patterns. Create atmosphere.

## Content Density Modes

| Density | Best for | Design |
|---------|----------|--------|
| Low / speaker-led | Public talks, keynote | One idea per slide, large type, 1-3 bullets max |
| High / reading-first | Reports, handouts, async review | Structured grids, 4-8 bullets, tighter spacing |

---

## Phase 0: Detect Mode

- **Mode A:** New Presentation → Phase 1
- **Mode B:** PPT Conversion → Phase 4
- **Mode C:** Enhancement → read existing file, check density, verify stage integrity

---

## Phase 1: Content Discovery

**Ask ALL questions together in one message:**
1. **Purpose:** Pitch deck / Teaching-Tutorial / Conference talk / Internal presentation
2. **Length:** Short 5-10 / Medium 10-20 / Long 20+
3. **Content:** All content ready / Rough notes / Topic only
4. **Density:** Low (speaker-led) / High (reading-first)

---

## Phase 2: Style Discovery

Generate 3 distinct single-slide HTML previews. Mix rules:
- 1 safe preset from `STYLE_PRESETS.md`
- 1+ bold template from `bold-template-pack/selection-index.json`
- 1 wildcard (custom design or second bold template)

**Mood → Preset Suggestions:**
| Mood | Presets |
|------|--------|
| Impressed/Confident | Bold Signal, Electric Studio, Dark Botanical |
| Excited/Energized | Creative Voltage, Neon Cyber, Split Pastel |
| Calm/Focused | Notebook Tabs, Paper & Ink, Swiss Modern |
| Inspired/Moved | Dark Botanical, Vintage Editorial, Pastel Geometry |

**NON-NEGOTIABLE:** Never render template names, paths, or internal labels on the slide itself.

Save to `.frontend-slides/slide-previews/` (style-a.html, style-b.html, style-c.html).

---

## Phase 3: Generate Presentation

**Before generating, read:**
- `html-template.md` — HTML architecture and JS features
- `viewport-base.css` — Mandatory CSS (include IN FULL)
- `animation-patterns.md` — Animation reference
- If bold template selected: read only that template's `design.md`

**Requirements:**
- Single self-contained HTML file, all CSS/JS inline
- Full `viewport-base.css` contents in `<style>` block
- Fontshare or Google Fonts — never system fonts
- `/* === SECTION NAME === */` comment blocks throughout
- Inline editing included by default (press E or hover top-left corner)

---

## Phase 4: PPT Conversion

1. `python scripts/extract-pptx.py <input.pptx> <output_dir>` (requires `pip install python-pptx`)
2. Confirm extracted content with user
3. Proceed to Phase 2 for style discovery
4. Generate HTML preserving all text, images, order, speaker notes

---

## Phase 5: Delivery

1. Delete `.frontend-slides/slide-previews/`
2. `open [filename].html`
3. Tell user: arrow keys/Space navigation, press E for inline editing, `:root` CSS variables for customisation

---

## Phase 6: Share & Export (Optional)

**Deploy to URL:** `bash scripts/deploy.sh <path-to-slide-folder-or-html>`
- Deploys to Vercel (free). Works on any device.
- Prereq: `npx vercel --version` + `npx vercel whoami`

**Export to PDF:** `bash scripts/export-pdf.sh <path-to-html> [output.pdf]`
- Uses Playwright (installs Chromium ~150MB on first run).
- Slides must use `class="slide"`.
- Smaller files: add `--compact` flag (1280×720).

---

## Supporting Files

| File | Purpose | When to Read |
|------|---------|---------------|
| `STYLE_PRESETS.md` | 12 curated visual presets | Phase 2 |
| `bold-template-pack/selection-index.json` | Compact bold template metadata | Phase 2 |
| `bold-template-pack/templates/*/preview.md` | Lightweight style cards | Phase 2 after shortlisting |
| `bold-template-pack/templates/*/design.md` | Full design system | Phase 3 after selection ONLY |
| `viewport-base.css` | Mandatory fixed-stage CSS | Phase 3 (always include in full) |
| `html-template.md` | HTML structure, JS features | Phase 3 |
| `animation-patterns.md` | CSS/JS animation snippets | Phase 3 |
| `scripts/extract-pptx.py` | PPT content extraction | Phase 4 |
| `scripts/deploy.sh` | Deploy to Vercel | Phase 6 |
| `scripts/export-pdf.sh` | Export to PDF | Phase 6 |
