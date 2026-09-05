# Claude Skills — by Shrinivas Ramaprasad

A growing library of reusable Claude skills and Claude Code skills.
All skills live under a single directory: `skills/[skill-name]/SKILL.md`.

---

## Skill Index

| Skill | Path | Type | Purpose |
|---|---|---|---|
| rtcfrr-prompt | `skills/rtcfrr-prompt/` | Claude Skill | Build structured AI prompts using the R·T·C·F·R·R framework — CV, LinkedIn, outreach, CareerForge |
| json-prompt-builder | `skills/json-prompt-builder/` | Claude Skill | Build JSON prompts for AI workflows, n8n nodes, and automation pipelines |
| n8n+claudedesktop | `skills/n8n+claudedesktop/` | Claude Skill | Connect n8n to Claude Web and Desktop — OS-specific config paths, token setup, workflow publishing, troubleshooting |
| canvas-design | `skills/canvas-design/` | Claude Skill | Create visual designs as .png and .pdf using design philosophy methodology |
| mcp-builder | `skills/mcp-builder/` | Claude Skill | Build MCP servers in Python (FastMCP) or TypeScript |
| skill-creator | `skills/skill-creator/` | Claude Skill | Create, iterate, test, and optimise new Claude skills — includes eval framework |
| uae-cv-builder | `skills/uae-cv-builder/` | Claude Skill | Build UAE/GCC ATS-optimised executive resumes for Program Manager, PMO, IT Director roles |
| skeptical-vc-red-team | `skills/skeptical-vc-red-team/` | Claude Skill | Universal VC-style red team for any product idea, business concept, or brainstorm |
| frontend-slides | `skills/frontend-slides/` | Claude Skill | Create animation-rich HTML presentations from scratch or by converting PPT/PPTX |
| github-content-fetcher | `skills/github-content-fetcher/` | Claude Skill | Fetch, classify, and save content from any GitHub URL into the correct local folder |
| growth-loop-sparring | `skills/growth-loop-sparring/` | Claude Skill | Evaluate and red-team growth loops — generates candidates, stress-tests each, picks the winner |
| aris-arxiv | `skills/aris-arxiv/` | Claude Code Skill | Search and download academic papers from arXiv |
| aris-auto-review-loop | `skills/aris-auto-review-loop/` | Claude Code Skill | Autonomous multi-round research review loop via Codex or manual reviewer |
| aris-idea-discovery | `skills/aris-idea-discovery/` | Claude Code Skill | Full idea discovery pipeline from a research direction to validated, pilot-tested ideas |
| aris-kill-argument | `skills/aris-kill-argument/` | Claude Code Skill | Adversarial attack-defense review for theory papers — find the worst-case rejection argument |
| aris-paper-writing | `skills/aris-paper-writing/` | Claude Code Skill | Full paper writing pipeline from narrative report to submission-ready PDF |
| aris-research-pipeline | `skills/aris-research-pipeline/` | Claude Code Skill | End-to-end autonomous research pipeline: idea discovery → experiments → paper |
| aris-research-review | `skills/aris-research-review/` | Claude Code Skill | Deep critical review via external reviewer backend (Codex or manual) |
| aris-semantic-scholar | `skills/aris-semantic-scholar/` | Claude Code Skill | Search published papers via Semantic Scholar API (IEEE, ACM, Springer) |
| karpathy-guidelines | `skills/karpathy-guidelines/` | Claude Code Skill | Behavioral guidelines to reduce common LLM coding mistakes |
| email-sequence | `skills/email-sequence/` | Claude Skill | Design multi-email sequences — welcome, nurture, re-engagement, onboarding, win-back |
| analytics-tracking | `skills/analytics-tracking/` | Claude Skill | Set up and audit analytics tracking — GA4, GTM, UTM strategy, event naming, tracking plans |
| ab-test-setup | `skills/ab-test-setup/` | Claude Skill | Design and run A/B tests — hypothesis framework, sample size, and a full growth experimentation program |
| free-tool-strategy | `skills/free-tool-strategy/` | Claude Skill | Plan and evaluate free marketing tools (engineering-as-marketing) for lead gen and SEO |
| launch-strategy | `skills/launch-strategy/` | Claude Skill | Plan product launches and feature announcements — ORB framework, five-phase launch, Product Hunt playbook |
| aso-audit | `skills/aso-audit/` | Claude Skill | Audit App Store and Google Play listings against ASO best practices with scored, prioritised fixes |
| form-cro | `skills/form-cro/` | Claude Skill | Optimise lead capture, contact, demo, and checkout forms for higher completion rates |
| paid-ads | `skills/paid-ads/` | Claude Skill | Plan and optimise paid ad campaigns across Google, Meta, LinkedIn, X, and TikTok |
| competitor-alternatives | `skills/competitor-alternatives/` | Claude Skill | Build competitor comparison and alternative pages for SEO and sales enablement |
| page-cro | `skills/page-cro/` | Claude Skill | Analyse and improve conversion rates on homepages, landing pages, pricing, and feature pages |
| tailored-resume-generator | `skills/tailored-resume-generator/` | Claude Skill | Tailor resumes to a specific job description with ATS keyword optimisation |
| knowledge-base-health-check-skill | `skills/knowledge-base-health-check-skill/` | Claude Skill | Audit a Claude-managed knowledge base — auto-fixes drift, drafts new articles, flags judgement calls |
| meeting-insights-analyzer | `skills/meeting-insights-analyzer/` | Claude Code Skill | Analyse meeting transcripts for communication patterns, conflict avoidance, and speaking ratios |
| lead-research-assistant | `skills/lead-research-assistant/` | Claude Code Skill | Identify and score high-fit leads from a product/service description, with contact strategies |
| langsmith-fetch | `skills/langsmith-fetch/` | Claude Code Skill | Debug LangChain/LangGraph agents by fetching and analysing LangSmith execution traces |
| content-research-writer | `skills/content-research-writer/` | Claude Code Skill | Collaborative writing partner — outlining, research citations, hook improvement, section feedback |
| video-import-public | `skills/video-import-public/` | Claude Code Skill | Upload a local video or hosted URL straight into a new Descript project — first step of the reel pipeline |
| content-gold-miner-public | `skills/content-gold-miner-public/` | Claude Code Skill | Mine a long already-filmed video for the 3–5 strongest reel ideas, each with hook line and timecode |
| script-prep-public | `skills/script-prep-public/` | Claude Code Skill | Turn one already-filmed, already-scripted piece into a final approved script from the best takes |
| video-editing-public | `skills/video-editing-public/` | Claude Code Skill | Cut and caption talking-head reels in Descript via the Descript MCP, from an approved keep-script |
| watch | `skills/watch/` | Claude Code Skill | Watch a video (URL or local path) via yt-dlp/ffmpeg and answer questions about what's in it |
| social-content | `skills/social-content/` | Claude Skill | Create, schedule, and repurpose social content for LinkedIn, X, Instagram, TikTok, and Facebook |
| signup-flow-cro | `skills/signup-flow-cro/` | Claude Skill | Optimise signup, registration, and trial activation flows |
| schema-markup | `skills/schema-markup/` | Claude Skill | Add and fix JSON-LD structured data for rich results — FAQ, product, review, breadcrumb schema |
| referral-program | `skills/referral-program/` | Claude Skill | Design and analyse referral, affiliate, and word-of-mouth programs |
| product-marketing-context | `skills/product-marketing-context/` | Claude Skill | Foundation skill — builds `.agents/product-marketing-context.md` that other skills read for product/ICP/positioning context |
| popup-cro | `skills/popup-cro/` | Claude Skill | Design and optimise popups, modals, overlays, and announcement banners |
| paywall-upgrade-cro | `skills/paywall-upgrade-cro/` | Claude Skill | Design in-app paywalls, upgrade screens, and upsell modals |
| pricing-strategy | `skills/pricing-strategy/` | Claude Skill | SaaS pricing, packaging, and monetisation strategy — tiers, value metric, willingness to pay |
| onboarding-cro | `skills/onboarding-cro/` | Claude Skill | Optimise post-signup onboarding, activation rate, and time-to-value |

---

## Folder Structure

```
claude-skills/
└── skills/
    └── [skill-name]/
        └── SKILL.md        ← Skill definition (frontmatter + instructions)
```

---

## How to Install a Skill

### Claude Skills (claude.ai)
1. Download the `SKILL.md` from the skill folder
2. Go to Claude → Settings → Skills → Add skill
3. Upload the `SKILL.md` file
4. Claude will automatically trigger it based on the description in the frontmatter

### Claude Code Skills
1. Download the `SKILL.md` from the skill folder
2. Place it in your project's `.claude/skills/` directory or follow Claude Code skill installation docs

---

## Frameworks Used

- **R·T·C·F·R·R** — Role · Task · Context · Format · Rules · Review (universal prompt framework)
- **R·T·C·S·F·R·E·V** — Extended for JSON / structured output tasks
- **Conversational Guide** — Plain text step-by-step guide delivered conversationally in chat
- **Visual Art** — Design philosophy methodology for museum-quality visual output
- **Dev Guide** — Step-by-step development framework for MCP server creation
- **Meta Skill** — Skills about creating and optimising other skills
- **Executive Resume** — UAE/GCC executive ATS resume builder with recruiter scanning optimisation
- **Red Team Framework** — Universal skeptical VC pressure-testing framework for product ideas and brainstorms

---

*Maintained by Shrinivas Ramaprasad | Built for CareerForge automations, n8n workflows, and AI agent pipelines.*
