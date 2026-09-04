---
name: script-prep-public
description: >-
  Turns an already-filmed single piece — a reel you scripted, or a 5–10 minute
  single-topic talk (even a messy ramble) — into a final approved script by finding the
  best take of each line and assembling them verbatim with precise timecodes. If it's an
  unstructured ramble, it finds the story/arc first. The idea is already decided — this
  does NOT mine long footage for multiple ideas (that's content-gold-miner-public) and does NOT
  cut or caption. TRIGGER on "find my best takes," "I scripted this," "help me build the
  final script," "I rambled about one thing — pick the best version," "prep my reel
  script," "which take is best," or any variation of selecting the cleanest takes or
  forming the final script from ONE intended piece. Reads your creator profile
  (audience, voice, goal) from your Claude Project, or runs a quick setup if none.
---

# Script Prep (Public)

For footage that is ONE intended piece, filmed with many retakes — find the best take of each line and form the final script, ranked for *your* audience and voice. (Public version.)

## What this skill is (and how it differs from content-gold-miner-public)

Use this when you already know the piece you're making and just need the best version of it assembled from the raw takes. Three cases:

- **Fully scripted** — you wrote a script and delivered it to camera (usually many times).
- **Single-topic talk** — you talked through one piece for ~5–10 minutes without a written script, but with one clear subject in mind.
- **Unstructured ramble** — you just talked, with a rough topic but no structure (common, and totally fine). Here the skill FINDS the arc first (see below), then assembles.

Either way, the idea is DECIDED. The job is convergent: find the best version of a KNOWN piece. Reach for `content-gold-miner-public` instead when the question is "what reels are hiding in this long footage?" — that's the divergent job.

This does NOT: mine for multiple ideas, write new copy from scratch, or cut/caption (that's your editing step in Descript).

## First: your creator profile (read this before forming anything)

The skill looks for a **creator profile** — ideally saved once in your **Claude Project** (short version in the Project's custom instructions; sample posts in Project knowledge) so you set it up once.

The profile holds:
1. **Your audience** — who you make content for: role, stage, the problem they're solving.
2. **What you do** — what you help people with, in a sentence.
3. **Your content goal** — visibility, engagement, leads, or sales.
4. **Your voice** — a few words on how you sound, or 2–3 sample posts that sound like you. (This matters most here — it's how the skill knows which takes sound like *you*.)

**If no profile exists yet,** ask those four questions once, use them for this session, and tell the person to save them into their Project. Never invent a profile — ask.

> **Worked example of a filled profile:** *Audience: expert service-providers/coaches who are great at their craft but hidden online. What I do: teach storytelling, thought leadership, and sales through their own content. Goal: leads. Voice: warm, direct, a little funny, story-first.* (Replace with yours.)

## Inputs

1. **The recording** — a Descript project/composition (or any recording with a transcript). `get_project` → `export_transcript` with fine-grained timecodes. **Read the ENTIRE transcript** before selecting. (Too long for one read? Read in chunks or have a subagent extract — cover 100%.)
2. **The written script, if there is one** — then the job is matching each scripted line to its best on-tape take.
3. **Target length** (ask once if not given).

## If it's an unstructured ramble: FIND THE ARC FIRST

When the recording has no structure — someone just talked for 5–10 minutes about a topic — do NOT jump straight to take-selection. First find the story buried in it:

1. Read the whole transcript.
2. Surface the **1–2 real through-lines** in it — the strongest idea or story arc that's actually there (use the same taste as content-gold-miner-public: tension, proof, novelty, audience fit).
3. **Name the strongest arc** and propose its shape (a simple hook → point → payoff, or problem → turning point → lesson — whatever the material actually supports). For someone who can't write a script, *"here's the story I see in your ramble"* is the whole value.
4. Get a quick yes on the arc, THEN assemble the best takes into it.

If there's genuinely no usable arc, say so plainly rather than forcing one.

## The core job: best-take selection

This is the heart of the skill.

- **For each line/beat, find the single cleanest, best-delivered take.** "Best" = clear delivery, full sentence (no dropped openers or clipped endings), right energy, their natural rhythm and voice.
- **Surface close calls — don't silently decide.** When two takes are both strong, present both with timecodes and a one-line trade-off so the creator picks ("the 2:14 take is cleaner; the 6:30 take has more energy"). Their taste is the tiebreaker.
- **Fully-scripted footage:** match each scripted line to its best take. Flag any scripted line with NO clean take — that's a pickup/re-record, not something to fudge with a weak take or (never) synthetic audio.
- **Single-topic talk / ramble:** more "forming" to do — order, what to keep, what to trim for length — but all WITHIN the one piece. Offer structural options; don't impose them.

## Offer options — do NOT prescribe taste

A single reel's edit is NOT a universal rule. One creator might lead with the payoff, drop the procedural "how," and end on emotion — but that's right for THAT reel, not always. So:

- **Follow their script and their intent.** If they wrote a hard CTA, keep it. If the step-by-step "how" earns its place, keep it. Do NOT quietly cut the "how" or soften a CTA.
- **Surface choices; let them decide.** "Open on the payoff or stay chronological?" "Keep the step-by-step, or trim it?" Offer; don't default.
- The skill's job is to execute their intent and present good options — never to editorialize their piece into a formula.

## The hook (optional)

When they want help choosing the strongest opening, evaluate the candidate lines that ALREADY EXIST in the footage against proven hook frameworks (a "hook-generator" skill, if available, is great for this), and rank the best on-tape scroll-stoppers (verbatim + timecode + why). Recommend one, but let them choose.

- **Real on-tape words only.** A talking-head reel can only use what they actually said. Hook help here is a RECOMMENDATION about existing takes, or — clearly labeled — a line to RE-FILM as a pickup. Never splice in synthetic/generated text.
- A payoff/result line is often the strongest hook even if it was said late in the recording — pulling it to the front is fair game.

## Per-piece intake (Tier 2 — quick)

Confirm (ask only what isn't obvious): what's the ONE thing the viewer should take away, anything that must stay (a CTA, a key moment), and target length.

## Form the final script (always present for approval)

- Build it ONLY from real on-tape lines — verbatim, retakes/false starts stripped. Never invent or paraphrase their words.
- **Tag each line with PRECISE in/out source timecodes** (start AND end, e.g. `[0:12.0–0:16.4]`). This is the handoff that lets the editing step do a fast deterministic trim instead of re-searching the retakes. When a line is stitched from more than one take, list every source range in order.
- Flag any connective words NOT on tape so they can approve or kill them. Default to fully verbatim.
- Present the complete script, with close-call alternatives noted, and wait for approval. Iterate as they edit.
- Never hand off to the editing step from an unapproved script.

## Hand-off

Once the final timecoded script is approved, point them to the editing step — "cut this in Descript (remove retakes, close word gaps, add captions)." If they have a video editing skill, offer to continue there. Only proceed with an approved script in hand; never cut inside this skill.

## Relationship to other skills

- Convergent sibling of **`content-gold-miner-public`** (divergent: mine long footage for the reels hiding in it). Same DNA: transcript read, retake-stripping, verbatim-only, precise-timecode handoff, never-cut-from-an-unapproved-script.
- Hands the approved, timecoded script off to your editing step in Descript (or a video editing skill).

## Change log

- 2026-06-29: Public version — de-personalized from an internal creator-specific skill. Now reads a CREATOR PROFILE (audience / what-you-do / goal / voice) from your Claude Project, or runs a quick setup if none exists. ADDED a "find the arc first" step for unstructured rambles (people who just talk 5–10 min with no script). Generic examples; soft handoff to "cut it in Descript." The core (best-take selection, surface close calls, offer-options-don't-prescribe, precise-timecode final script) is unchanged.
- 2026-07-06: Renamed skill from `script-reel-prep` to `script-prep-public` (cross-references updated to the new public skill names).
