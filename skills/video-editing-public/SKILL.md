---
name: video-editing-public
description: >-
  Cuts and captions talking-head reels in Descript via the Descript MCP, from an
  APPROVED keep-script (normally from script-prep-public or content-gold-miner-public). Use
  whenever editing, tightening, restructuring, or captioning a talking-head video.
  TRIGGER on "edit my reel," "clean up this talking head," "remove the retakes,"
  "tighten this," "cut this to X seconds," "remove word gaps," "add my captions," "cut
  this from the approved script," or any variation of cutting/captioning a talking-head
  recording. Applies the creator's caption style from their profile, or asks once. NOTE:
  this is the most credit-hungry skill of the set — needs a paid Descript plan with AI
  credits, and (for the local upload step) Code/Cowork.
---

# Video Editing (Public)

Cut + caption a talking-head reel in Descript from an approved keep-script, in the creator's own style. (Public version.)

## Non-negotiable working principles

1. **Never cut from an unapproved script.** Always build from a full keep-script the creator has approved (normally from `script-prep-public` or `content-gold-miner-public`). If you land here without one, stop and get one approved first.
2. **The creator's edits are the source of truth.** Build forward from the current state; never roll back; never re-add words/lines they removed. If you think something should come back, ASK — don't just do it. (And never run a job on a composition they may be mid-editing — confirm first; if you notice an unexpected change like a duration drop, ask whether they made it before acting.)
3. **Never change their words.** When tightening, only shorten silences and breaths. Don't alter/add/reorder/remove words unless they ask. (Filler removal is the one exception, only when asked.)
4. **Verify after every agent pass — and never trust the agent's own readback.** The Descript agent drifts: it re-picks takes, drops words (especially sentence openers), duplicates phrases, swaps takes, over-includes tangents, and sometimes inserts synthetic placeholder text. After every `prompt_project_agent` job, run `export_transcript` and check the words and duration. It re-pastes earlier summary/inspection tables verbatim, so a confident readback proves nothing. Confirm visual things (captions, highlight, shadow) BY EYE.
5. **Real audio only.** Never accept synthetic / scratch / TTS placeholder text — it leaves a silent gap on screen.
6. **Prefer continuous real takes over splices.** When a line must be rebuilt, favor one clean continuous take; if a splice is unavoidable, flag the spot for an ear-check.
7. **Make corrections surgically.** Fix one specific thing per pass, then verify. Broad "redo everything" prompts are where the agent does the most damage. (Over-fixing a minor issue often introduces a new one — when a cut is good enough, stop.)
8. **Build each reel as its own NEW composition.** Never edit the raw source composition. Name each clearly. After building, `get_project` and check for a stray duplicate composition the agent may have spawned; flag any for in-app deletion.

## Build-in-public / process footage (special case)

When the reel is the creator narrating themselves DOING something in real time (rather than a teaching reel with clean claims), the "strip everything that isn't a polished claim" instinct is WRONG:
- Keep the process narration and any spoken timestamps — they're the content; they make it organic and prove the speed.
- Preserve chronological order; don't reorder into a hook → point → payoff arc.
- Don't force a hook; open where the session really opens.
- Don't over-sanitize — lower-polish, mid-thought takes can sound more human than the single cleanest take.
- Keep the honest, in-progress ending.
- If a stretch sags ("tooly" explainer), cut it rather than leaving it with a note.

For a normal teaching reel, the standard cut rules below apply.

## The cut (transcript-based editing)

**Cut from timecodes, not from text — the #1 speed lever.** If the approved script carries precise per-line in/out timecodes (it should — `script-prep-public`/`content-gold-miner-public` emit them), drive the FIRST cut as a deterministic trim: hand the agent the exact source ranges to keep, in order, and tell it plainly **"trim to these exact ranges; do not re-select takes, do not search for a cleaner version."** This turns the build from a slow fuzzy search across every retake (drift-prone) into a near-mechanical cut. Only fall back to locating lines by text when a script arrives without timecodes.

**For long scripts, build in 2–3 sections** rather than one monolithic prompt (the longest jobs drift the most).

**Pre-empt the dropped sentence-opener** (the agent's most reliable bug): when splicing it drops the first word(s) of a sentence. Put an explicit line in the build prompt: *"keep each sentence's opening words — do not drop sentence openers."* Then verify by transcript and restore any that broke. The agent will also claim it fixed these when it didn't — confirm in the transcript.

1. Read the full transcript (`export_transcript` with paragraph timecodes) so you can locate every approved line and its cleanest take.
2. Map the structure against the approved keep-script. Your job is to find each approved line, not re-write it.
3. **Remove all retakes and false starts** — keep the single cleanest, most complete take of each idea. (Exception: build-in-public footage — don't over-sanitize.)
4. **(Text-located cuts only)** Expect to re-cut with a named kill list: when you located lines by TEXT (no timecodes), the first cut usually over-includes — run a trim pass listing the EXACT phrases to remove. A timecode-driven trim usually lands in one pass and skips this.
5. **Assembling from scattered moments / reordering:** label each segment with its source timecode AND state plainly "this is NOT the order in the recording — reorder to match this sequence." (Not for build-in-public footage, which stays chronological.)
6. **Remove ALL word gaps — target zero.** Fully contiguous, wall-to-wall speech; close every gap (breaths, pauses, splice seams) to ~0.0s. Do NOT clip the start/end of any word. Trim dead air before the first and after the last word.
7. **Cut to the target length** the creator gives. If it runs long, trim lowest-priority content first and say what you trimmed.
8. **One reel = one idea.** If a script carries two distinct ideas, flag it and split (with their okay) into separate reels.

## Token / credit efficiency (each agent job costs — minimize them)

Every `prompt_project_agent` job spends Descript AI credits and several minutes; `export_transcript` and `get_project` are FREE. So: minimize the number of AGENT jobs, verify with the free read tools, and make each job deterministic (timecodes, not search).

- **Combine the cut and the zero-gap into ONE job** when trimming to precise timecodes — don't run cut → verify → gap as separate paid passes.
- **Verify with the FREE tools, never with an agent pass.** Never spend a separate agent job just to "inspect" or "report values" — ask for raw values *inside* the edit job you're already running.
- **Captions are a separate step — only run the caption pass when asked.** Many creators caption in-app themselves.
- **Pre-empt fixes** (timecodes + keep-openers + don't-re-select) so the first cut is right and you skip corrective passes.
- **Budget credits for the whole flow up front.** A mid-job "Insufficient AI credits" failure burns credits on a partial job AND leaves a partial composition. On a failed/partial build, resume by rebuilding INTO the existing partial composition (pass its `composition_id`, "rebuild this to match exactly") — no stray, no rework.

## Caption style (from the creator's profile)

**The caption look comes from the creator's saved style — there is no one fixed recipe.** Apply the creator's caption style: font + weight, size, box width (one word at a time vs. phrases), fill color, stroke/outline, drop shadow, placement, and whether there's an active-word highlight. If no style is saved, ask once, apply it, and suggest they save it to their profile/Project so every reel matches.

### Universal caption mechanics (apply to ANY style — these are Descript gotchas, not preferences)
- **Active-word highlight:** the "Karaoke: One word" preset ships with a cyan highlight box behind the active word (`playheadBackgroundColor [0,170,255]`). If the creator's style has NO highlight, you must explicitly set `playheadBackgroundColor` to fully transparent `[0,0,0,0]` on **every** caption pass — the agent leaves it on and reports "no background" while the cyan box is live.
- **Box width is the #1 caption failure point.** ALWAYS set the bounding box explicitly and VERIFY the returned px width. A wide/full-frame box shows 3–6 words even with future words transparent. One-word-at-a-time wants a narrow box (~155px).
- **The drop-shadow COLOR cannot be set via the API.** The agent can set shadow blur and offset, but NOT the color (backend-blocked / silently forced transparent). So an agent-applied shadow renders invisible even when "blur 30" reads correct. Getting the shadow is a **manual in-app step** (set the color in Descript's caption panel; best practice: set it once, save it as a caption preset, reuse across reels).
- **Font fallback:** confirm the chosen font actually renders. A default font like **"Lato"** showing up in the project's font list is a tell that a default caption template got applied somewhere — verify by eye.
- **Trust NO caption value from the agent readback** (font, box px, highlight, stroke, shadow, position) — confirm captions BY EYE before declaring them done.

### Worked example of a filled caption style (one creator's signature look)
*One word on screen at a time (past/future words fully transparent, alpha 0); NO active-word highlight (cyan cleared to `[0,0,0,0]`); narrow ~155px × ~110px box; font Fraunces, semi-bold (weight 600), not italic; size 90; white fill, fully opaque; stroke 0 (no outline); black drop shadow, blur 30, offset 0,0 (set in-app); centered, vertically middle (~51% y); captions span the entire video.* — That's one creator's style; replace it with the user's.

## Final check before declaring a reel done

Run this and report it honestly:
1. **Transcript** matches the approved script (verified via `export_transcript`, no drift, no over-included tangents).
2. **Gaps** at zero (confirmed by the duration drop / gap count).
3. **Captions** match the creator's style, the box width is verified in px, and the active-word highlight is cleared if their style has none — all confirmed BY EYE (not from the readback). The drop-shadow color needs their in-app step.
4. **Script quality** — single idea, real arc; no padding; every line lands. (If the approved script falls short, flag it back rather than rewriting it.)
5. **Strays** — `get_project` confirms no duplicate composition was spawned.

## Working with the Descript MCP

- **Lean flow:** `export_transcript` (FREE) → one combined job: trim to the approved timecodes AND remove word gaps → verify with `export_transcript`/`get_project` (FREE) → captions only if asked → verify by eye. ~1–2 agent jobs total.
- **`wait_for_job` may time out at the transport while the job keeps running.** Don't assume failure — use `list_jobs` to read live progress, then re-check until `job_state` is "stopped" with `result.status` "success." For long builds, kick it off and poll every few minutes (or background it) rather than watching it spin.
- Give the agent the **exact source timecode ranges** (or exact verbatim target text if no timecodes) and tell it not to re-plan or re-select takes.
- **Continue the same conversation across passes** with the returned `conversation_id`, but ignore its re-pasted stale tables — read only the new lines, or run a fresh inspection when you need a clean readback.
- To verify caption properties, ask for the **literal current raw values** (font, box px, `playheadBackgroundColor` with alpha, position %) — then still confirm by eye.
- `get_project` confirms duration, media, fonts, and compositions. Its composition list can be eventually-consistent — re-pull before assuming anything is missing.
- **Watch for "Insufficient AI credits"** — agent jobs can fail mid-batch.

## Getting footage into Descript

Use `video-import-public` (uploads any-size footage straight from your machine — needs Code/Cowork), or drag the file into the Descript desktop app, or import by URL (Dropbox/direct link). This skill assumes the footage is already in Descript.

## Relationship to other skills

- Fed an approved, timecoded keep-script by **`script-prep-public`** (one intended piece) or **`content-gold-miner-public`** (mined from long footage).
- Reads the creator's **caption style** from their profile/Project; applies it with the universal caption mechanics above.

## Change log

- 2026-06-29: Public version — de-personalized from an internal creator-specific skill. The hardcoded caption recipe became the CREATOR'S CAPTION STYLE (from their profile), with the universal caption mechanics (cyan highlight, box-width, shadow-via-app-only, font fallback, never-trust-the-readback) preserved and the signature recipe kept only as a worked example. The engine — cut-from-timecodes, token efficiency, verify-don't-trust-readback, new-composition, build-in-public — is unchanged. Stripped creator-specific reel references.
- 2026-07-06: Renamed skill from `talking-head-editing` to `video-editing-public` (cross-references updated to the new public skill names).
