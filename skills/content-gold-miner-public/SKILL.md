---
name: content-gold-miner-public
description: >-
  Mines a long, already-filmed video (a Descript project, webinar, masterclass,
  podcast, coaching call, or rambly talk) for the 3–5 strongest reel ideas already in
  it, ranked for YOUR ideal audience, each with the verbatim hook line and its timecode
  so it can be cut directly. It locates what's on tape and drafts the script for
  approval — it does NOT write new posts from scratch, and does NOT cut or caption.
  TRIGGER on "find the gold in this video," "mine this recording," "what reels are in
  here," "what's worth cutting from this," "find the hooks in this recording," or any
  variation of pulling reel-worthy moments out of a long raw video. Reads your creator
  profile (audience, voice, goal) from your Claude Project, or runs a quick setup if
  none.
---

# Content Gold Miner (Public)

Mine a long, already-filmed video for the reel gold that's already in it — ranked for *your* audience. (Public version.)

## What this skill is (and the boundary)

Use this when you have a long recording — a webinar, masterclass, podcast, coaching call, or a raw talk full of retakes — and you want to know which moments are worth turning into short-form reels. The footage already exists; the hook is already somewhere in the transcript. The job is to find it, say exactly where it is (with a timecode), and draft the full script before anything gets cut.

It does NOT:
- Write new posts, captions, carousels, threads, or emails from scratch (it locates what's already on tape; it doesn't repurpose into written formats).
- Cut, edit, or caption the video (that's your editing step in Descript).
- Add hashtags, graphics, or scheduling.

It stops at: "here are the best reel ideas already in this footage, here's the exact hook line and timecode for each, here are the supporting moments, and here's the full draft script for the one(s) you pick." Then it hands the script off.

## First: your creator profile (read this BEFORE scoring anything)

To judge what's reel-worthy, the skill needs to know who you are and who you're for. It looks for a **creator profile** — ideally saved once in your **Claude Project** (put the short version in the Project's custom instructions, and any longer assets like sample posts in Project knowledge) so you set it up once and every chat inside that Project can use it.

The profile holds:
1. **Your audience** — who you make content for: their role, their stage, and the problem they're trying to solve.
2. **What you do** — what you help people with, in a sentence.
3. **Your content goal** — mostly visibility, engagement, leads, or sales.
4. **Your voice** — a few words on how you sound (e.g. "warm and direct," "bold, a little funny"), or 2–3 sample posts that sound like you.

**If no profile exists yet,** ask those four questions once, use the answers for this session, and tell the person to save them into their Project so they never have to answer again. Never invent a profile — ask.

> **Worked example of a filled profile (so you know what "good" looks like):** *Audience: established service-providers, coaches, and course-creators who are great at their craft but invisible online and want to grow with content that sounds like them. What I do: teach storytelling, thought leadership, and sales through their own content. Goal: leads. Voice: warm, direct, a little funny, story-first.* (That's one creator's — replace it with yours.)

## Inputs

1. **The source video** — a long recording with a transcript. With the Descript MCP: `get_project` to find the composition, then `export_transcript` with paragraph timecodes. **Read the ENTIRE transcript before selecting anything.** (If it's too long for one read, read it in chunks or have a subagent extract the key moments verbatim — but cover 100%.)
2. **The creator profile** (above).
3. **This video's intent** (per-video — ask if not obvious): what's this recording about, who's this set of reels for, and is there a target reel length?

## First, identify the footage type

Before scoring, decide which kind of recording this is, because the definition of "gold" flips between them:

- **Teaching footage** — a training, talk, webinar, or coaching call with teachable snippets. Mine for the strongest standalone ideas; the standard scoring below applies.
- **Build-in-public / process footage** — the creator narrating themselves DOING something in real time (using a tool, running a workflow, building something). Here the gold is the real-time process story itself — see the special-case section. The polished-pitch instinct is WRONG for this footage.

If you're unsure, say which you think it is and why before drafting.

## The standard (taste)

Read everything first. Be ruthless — most of a raw recording is not gold.

Score every candidate moment on five factors. The first four are the taste filter; the fifth is the override:

1. **Tension** — friction, surprise, contradiction, or emotional charge.
2. **Proof** — specifics, numbers, stakes, lived experience, behind-the-scenes reality.
3. **Novelty** — challenges a common belief or says it in a fresh way.
4. **Self-contained** — because it's already filmed, the moment must work as a clip without heavy setup. Favor passages that cut into a reel with minimal stitching. (Build-in-public footage is the exception — a clip can need its chronological neighbors, and that's fine.)
5. **Audience fit (override)** — a moment only counts if it lands with the creator's ideal audience (from the profile). A clever idea that doesn't serve their audience is not gold here — cut it.

Reject on sight: generic advice, recap, vague "value," nice-sounding truths, repeated points, anything needing too much setup, anything merely "interesting."

**The hook is in the transcript.** For every idea, find the actual spoken line that is the scroll-stopping hook — verbatim, with timecode. Do not invent hooks; locate them. (Exception: build-in-public footage — don't force a punchy hook; the real opening line of the session is the opener, even if it's matter-of-fact.)

## When the footage is build-in-public / process narration (a different kind of gold)

Some recordings aren't a training with teachable snippets — they're the creator narrating themselves DOING something in real time. For this footage the gold is the OPPOSITE of what the polished-pitch instinct reaches for:

- **The process narration IS the content, not filler.** Keep the real-time play-by-play ("okay, here goes," "let's see what it does," "I'm going to go do X now") — it creates the live, documentary, watch-it-happen feel.
- **Spoken timestamps stay — they prove the claim.** Real-time markers ("it's 3:59," "okay, it's 4:38 now") prove speed honestly, better than any claim line.
- **Follow the chronology. Do NOT reorder into a marketing arc.** The sequence of actually doing the thing IS the story. Reassembling scattered cleanest-takes into a hook → point → payoff structure kills the realness. Draft a chronological keep-script of the real session.
- **Don't force a scroll-stop hook.** Open where the real session opens.
- **Lower-polish, mid-thought takes can beat the single cleanest take.** They sound human, not like a read.
- **Lean into the honest, in-progress ending.** Ending mid-process IS the credibility.
- **If a stretch is weak or "tooly," cut it — don't leave it with a note.**

When you suspect build-in-public footage, say so, draft a chronological keep-script of the real session, and tell the creator that's the approach.

## Per-video intake (Tier 2 — quick, each time)

Before drafting, confirm (ask only what isn't obvious from the footage/profile):
- **What's this video about**, in a line?
- **Who's this set of reels for**, and what's the ONE thing they should take away?
- **Anything that must stay** (a key moment, a CTA)?
- **Target reel length** (30 / 60 / 90s, or "as tight as it needs to be")?

## Output (3–5 ideas; fewer if the footage is thin)

For each idea, in priority order:

### Idea title
A short, sharp label for the reel.

### Why it lands for their audience
Two or three sentences naming the specific nerve it hits for the creator's ideal audience — what belief it challenges or what pain it speaks to. Be specific to their audience, not generic.

### The hook (already on tape)
Their exact words, verbatim, that open the reel — with the timecode. The line that stops the scroll, pulled straight from the footage. (For build-in-public footage, this is the real opening line, not an engineered hook.)

### Supporting moments
The other lines/segments that build the reel, each with a timecode, in the order they'd be cut. Enough that the editing step could assemble it directly. Keep excerpts in their natural language. For build-in-public footage, keep these chronological.

### Suggested shape
Rough length, and whether it's a strong standalone or fits a series.

## Draft script (always present before handoff)

After the creator picks the idea(s) they want, write the FULL proposed script for each one before any cutting begins. This is the script-approval step for the whole workflow.

- Build it ONLY from lines that already exist in the transcript — verbatim, with retakes and false starts stripped. Never invent or paraphrase their words.
- **Tag each line with PRECISE in/out source timecodes — a start AND end, e.g. `[0:12.0–0:16.4]`** — not an approximate "around 4:05." These exact ranges are what let the editing step do a fast deterministic trim instead of re-searching all the retakes. When a line is stitched from more than one take, list every source range in order. The take-selection (which retake is cleanest) is YOUR job here, done as cheap text analysis; lock it as numbers.
- When they want sentence-by-sentence footage, number each line as its own clip.
- Flag any connective words that are NOT on tape so they can approve or kill them. Default to fully verbatim.
- For build-in-public footage, draft the script in CHRONOLOGICAL order; don't over-sanitize.
- Present the complete script and wait for approval. Iterate as they give feedback.
- Never hand off to the editing step from an unapproved script.

## After the ideas

- **Throughlines:** 2–3 themes connecting multiple ideas, and where weaker ones could merge into one stronger reel.
- **Strongest single pick:** name the one reel you'd cut first and why.
- **Hand-off:** once a full draft script is approved, point them to the editing step — "cut this in Descript (remove retakes, close word gaps, add captions)." If they also have `script-prep-public` or a video editing skill, offer to continue there. Only proceed to editing with an approved script in hand; never cut inside this skill.

## Volume & honesty rules

- Let the footage dictate volume. If only two moments are truly strong, give two.
- If the footage is weak, say so plainly — don't manufacture gold that isn't there.
- No filler, no praise, no summary of the whole video.

## Relationship to other skills

- Pairs with **`script-prep-public`** (the convergent sibling: one intended piece → best takes + final script). Use gold-miner when the question is "what reels are hiding in this footage?"; use script-prep-public when you already know the one piece and just need the best version of it.
- Hands the approved, timecoded script off to your editing step in Descript (or a video editing skill).

## Change log

- 2026-06-29: Public version — de-personalized from an internal creator-specific skill. Now reads a CREATOR PROFILE (audience / what-you-do / goal / voice) from your Claude Project, or runs a quick setup if none exists, instead of a hardcoded ICP. Generic footage examples; per-video intake added; soft handoff to "cut it in Descript." The engine (5-factor scoring, footage-type detection, build-in-public rules, and the precise-timecode draft script) is unchanged.
- 2026-07-06: Renamed skill to `content-gold-miner-public` (cross-references updated to the new public skill names).
