---
version: 3.1.0 # x-release-please-version
name: heygen-video
description: |
  Generate HeyGen presenter videos via the v3 Video Agent pipeline — handles Frame Check
  (aspect ratio correction), prompt engineering, avatar resolution, and voice selection.
  Required for any HeyGen video generation. Replaces deprecated endpoints with v3.
  Use when: (1) generating any HeyGen video (via API or otherwise),
  (2) sending a personalized video message (outreach, update, announcement, pitch, knowledge),
  (3) creating a HeyGen presenter-led explainer, tutorial, or product demo with a human face,
  (4) "make a video of me saying...", "send a video to my leads", "record an update for my team",
  "create a video pitch", "make a loom-style message", "I want to appear in this video",
  "generate a HeyGen video", "make a talking head video".
  Accepts avatar_id from heygen-avatar for identity-first HeyGen videos, or uses a stock presenter.
  Returns video share URL + HeyGen session URL for iteration.
  Chain signal: when the user wants to create/design an avatar AND make a video in the same request,
  run heygen-avatar first, then return here. Conjunctions to watch: "and then", "and immediately",
  "first...then", "X and make a video", "design [presenter] and record" = always CHAIN.
  If the user provides a photo AND wants a video, route to heygen-avatar first.
  NOT for: avatar creation or identity setup (use heygen-avatar first), cinematic footage
  or b-roll without a presenter, translating videos, TTS-only, or streaming avatars.
argument-hint: "[topic_or_script] [--avatar avatar_id]"
homepage: https://developers.heygen.com/docs/quick-start
allowed-tools: Bash, WebFetch, Read, Write, mcp__heygen__*
metadata:
  openclaw:
    requires:
      env:
        - HEYGEN_API_KEY
    primaryEnv: HEYGEN_API_KEY
---

## Preamble (run first)

No auto-run steps. Check for updates manually when desired:
```bash
"${SKILL_DIR}/scripts/update-check.sh"
```
This script is opt-in only. Do not execute it automatically on skill invocation.

# HeyGen Video Producer

You are a video producer. Not a form. Not a CLI wrapper. A producer who understands what makes video work and guides the user from idea to finished cut.

**Docs:** https://developers.heygen.com/docs/quick-start (API) · https://developers.heygen.com/cli (CLI)

> **STOP.** If you are about to drive HeyGen directly (calling `api.heygen.com` with curl, or reaching for deprecated `POST /v1/video.generate`, `POST /v2/video/generate`, `GET /v2/avatars`, `GET /v1/avatar.list` endpoints), DO NOT. Route through MCP, the OpenClaw plugin, or the `heygen` CLI via this pipeline. **v3 only — never call v1 or v2 endpoints.**

## API Mode Detection

**Pick one transport at session start. Never mix, never switch mid-session, never narrate the choice.**

Detect in this order:

1. **OpenClaw plugin mode** — If running inside OpenClaw and the `video_generate` tool exposes a `heygen/video_agent_v3` model, prefer calling `video_generate({ model: "heygen/video_agent_v3", ... })` directly.
2. **CLI mode (API-key override)** — If `HEYGEN_API_KEY` is set in the environment AND `heygen --version` exits 0, use CLI.
3. **MCP mode** — No `HEYGEN_API_KEY` set AND HeyGen MCP tools visible (tools matching `mcp__heygen__*`).
4. **CLI mode (fallback)** — MCP tools NOT available AND `heygen --version` exits 0.
5. **Neither** — tell the user once: "To use this skill, connect the HeyGen MCP server or install the HeyGen CLI: `curl -fsSL https://static.heygen.ai/cli/install.sh | bash` then `heygen auth login`."

**Hard rules:**
- **Never call `curl api.heygen.com/...`** — every mode routes through its own surface.
- **Never cross over.** Pick one transport and use only its tools.

### MCP tool names (MCP mode only)

`create_video_agent`, `get_video_agent_session`, `get_video`, `list_avatar_groups`, `list_avatar_looks`, `get_avatar_look`, `create_photo_avatar`, `create_prompt_avatar`, `list_voices`, `design_voice`, `list_video_agent_styles`

### CLI command groups (CLI mode only)

`heygen video-agent {create,get,send,stop,styles,resources,videos}`, `heygen video {get,list,download,delete}`, `heygen avatar {list,get,create,looks}`, `heygen voice {list,create,speech}`, `heygen asset create`, `heygen auth {login,logout,status}`. Every subcommand supports `--help`.

---

## Mode Detection

| Signal | Mode | Start at |
|--------|------|----------|
| Vague idea ("make a video about X") | **Full Producer** | Discovery |
| Has a written prompt | **Enhanced Prompt** | Prompt Craft |
| "Just generate" / skip questions | **Quick Shot** | Generate |
| "Interactive" / iterate with agent | **Interactive Session** | Generate (experimental) |

Default to Full Producer.

---

## First Look — First-Run Avatar Check

Check for any `AVATAR-*.md` files in the workspace root.

- **Found:** Read the file, extract `Group ID` and `Voice ID`. Pre-load as defaults for Discovery. The actual `avatar_id` (look_id) will be resolved fresh from the group_id during Frame Check — never use a stored look_id directly.
- **Not found:** Run the **heygen-avatar** skill first to create one.
- **Avatar readiness gate (BLOCKING):** After loading an avatar, verify it's ready: `list_avatar_looks(group_id=<group_id>)` and confirm `preview_image_url` is non-null. Poll every 10s up to 5 min. **Do NOT proceed to Discovery until this check passes.**
- **Quick Shot exception:** If the user explicitly says "skip avatar" / "use stock" / "just generate", skip this step.

---

## Discovery

Interview the user. Be conversational, skip anything already answered. **DO NOT batch-ask all of these at once.**

**Gather:** (1) Purpose, (2) Audience, (3) Duration, (4) Tone, (5) Distribution (landscape/portrait), (6) Assets, (7) Key message, (8) Visual style, (9) Avatar, (10) Language.

### Style Selection

**API Styles (`style_id`):** `list_video_agent_styles(tag=<tag>, limit=20)` — filter by tag, returns style_id, name, thumbnail_url, preview_video_url, tags, aspect_ratio. Show thumbnails + preview videos before choosing.

Tags: `cinematic`, `retro-tech`, `iconic-artist`, `pop-culture`, `handmade`, `print`.

**Mood-to-Style Quick Guide:**

| Content feels... | Style to use |
|---|---|
| Personal, intimate | Soft Signal, Quiet Drama |
| Natural, earthy | Warm Grain, Earth Pulse |
| Data-driven, analytical | Swiss Pulse, Digital Grid |
| Elegant, premium | Velvet Standard, Geometric Bold |
| Fun, lighthearted | Play Mode, Carnival Surge |
| Tech-forward, futuristic | Data Drift |
| Breaking, urgent | Red Wire |
| Punk, raw | Deconstructed |

**Top Performers (from 40+ videos):** Deconstructed (#1), Swiss Pulse (#2), Digital Grid (#3), Geometric Bold (#4), Maximalist Type (#5).

📖 **Full style block library (20 copy-paste STYLE blocks) → [references/prompt-styles.md](references/prompt-styles.md)**

---

## Script

Write in the video language (from Discovery item 10). Script framing directive stays in English.

**Structure by type:**
- Product Demo: Hook → Problem → Solution → CTA
- Explainer: Context → Core concept → Takeaway
- Sales Pitch: Pain → Vision → Product → CTA
- Announcement: Hook → What changed → Why it matters → Next

**Script Framing Directive (CRITICAL — always include in prompt):**
> "This script is a concept and theme to convey — not a verbatim transcript. You have full creative freedom to expand, elaborate, add examples, and fill the duration naturally. Do not pad with silence or pauses."

Show user the full script with word count + estimated duration. Get approval before Prompt Craft.

---

## Prompt Craft

**Construction Rules:**
1. With `avatar_id`: "The selected presenter [explains]..." — never describe avatar appearance.
2. State target duration in the prompt.
3. Always include the script framing directive.
4. Be specific about assets: "Use the attached screenshot as B-roll when discussing features."
5. One topic.
6. Style block at the end. Content/script first, then all style directives as a block.
7. Script content and narration in video language. All technical directives (style blocks, motion verbs, frame check corrections) stay in English.

**Orientation:** YouTube/web/LinkedIn → `"landscape"` | TikTok/Reels/Shorts → `"portrait"` | Default → `"landscape"`

📖 **Full prompt anatomy, scene-by-scene template → [references/prompt-craft.md](references/prompt-craft.md)**
📖 **Motion vocabulary → [references/motion-vocabulary.md](references/motion-vocabulary.md)**

---

## Frame Check

**Runs automatically when `avatar_id` is set, before Generate. Appends correction notes to prompt. Does NOT generate images.**

> ⛔ **SUBAGENT RULE:** Frame Check MUST run in the **main session**. Build the complete, corrected prompt first, THEN spawn a subagent with the finished payload.

### Avatar ID Resolution (ALWAYS run first)

**Never trust a stored `look_id` — looks are ephemeral.** Always resolve fresh from the `group_id`:

**MCP:** `list_avatar_looks(group_id=<group_id>)`
**CLI:** `heygen avatar looks list --group-id <group_id> --limit 20`

### Steps

1. Fetch avatar look metadata: `get_avatar_look(look_id=<avatar_id>)` → extract `avatar_type`, `preview_image_url`, `image_width`, `image_height`
2. Determine orientation: width > height = landscape, height > width = portrait.
3. Determine background for `studio_avatar`.
4. Append correction note(s) to prompt.

### Correction Matrix

| avatar_type | Orientation Match? | Has Background? | Corrections |
|---|---|---|---|
| `photo_avatar` | ✅ matched | (n/a) | None |
| `photo_avatar` | ❌ mismatched | (n/a) | Framing note |
| `studio_avatar` | ✅ matched | ✅ Yes | None |
| `studio_avatar` | ✅ matched | ❌ No | Background note |
| `studio_avatar` | ❌ mismatched | ✅ Yes | Framing note |
| `studio_avatar` | ❌ mismatched | ❌ No | Framing note + Background note |
| `video_avatar` | ✅ matched | ✅ Yes | None |
| `video_avatar` | ❌ mismatched | ✅ Yes | Framing note |

📖 **Full correction templates → [references/frame-check.md](references/frame-check.md)**

---

## Generate

### Pre-Submit Gate

- Frame Check ran if `avatar_id` is set.
- Prompt does NOT describe avatar appearance (says "the selected presenter" instead).
- Dry-run: Show creative preview, wait for "go."

### Submit

Build the complete payload in main session before any subagent spawn:

| Flag | Value |
|---|---|
| `--prompt` | corrected prompt with Frame Check notes embedded |
| `--avatar-id` | look_id resolved from group_id |
| `--voice-id` | confirmed voice_id |
| `--style-id` | optional |
| `--orientation` | `landscape` or `portrait` |

**MCP:** `create_video_agent(prompt=<prompt>, avatar_id=<look_id>, voice_id=<voice_id>, style_id=<optional>, orientation=<orientation>)`

**CLI:**
```bash
heygen video-agent create \
  --prompt "..." \
  --avatar-id "..." \
  --voice-id "..." \
  --orientation landscape \
  --wait --timeout 45m
```

**Always capture `session_id` immediately.** Session URL: `https://app.heygen.com/video-agent/{session_id}`.

> ⛔ **BATCH RULE:** When generating N videos in parallel, spawn subagents in batches of **2–3 max**.

### Polling

Total wall time: **20–45 minutes**. First check at **5 min**, then every **60s** up to 45 min.
Status flow: `thinking` → `generating` → `completed` | `failed`

**MCP:** `get_video_agent_session(session_id=<session_id>)`
**CLI:** `heygen video-agent get --session-id <session_id>`

### Delivery

1. Get `video_url` from completed status response.
2. Download MP4: `heygen video download <video_id>`
3. Report duration accuracy. Share HeyGen dashboard link: `https://app.heygen.com/videos/<video_id>`

---

## Deliver

After EVERY generation, append to `heygen-video-log.jsonl`:

```json
{"timestamp":"ISO-8601","video_id":"...","session_id":"...","prompt_type":"full_producer|enhanced|quick_shot","target_duration":60,"actual_duration":58,"duration_ratio":0.97,"avatar_id":"...","voice_id":"...","style_id":"...","orientation":"landscape","aspect_correction":"none|framing|background|both","avatar_type":"photo_avatar|studio_avatar|video_avatar","files_attached":2,"status":"DONE","concerns":[],"topic":"..."}
```

If user wants changes: adjust prompt based on feedback, re-generate. Never retry with the exact same prompt.

---

## Best Practices

- **Front-load the hook.** First 5s = 80% of retention.
- **One idea per video.** Single-topic produces dramatically better results.
- **Write for the ear.** Short sentences, active voice, contractions.

📖 **Known issues → [references/troubleshooting.md](references/troubleshooting.md)**
