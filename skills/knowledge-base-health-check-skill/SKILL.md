---
name: knowledge-base-health-check-skill
description: "Audits one knowledge base in a Claude-managed knowledge base system. Auto-fixes routine drift (writing-rules violations, broken backlinks, em-dash bullet patterns, RAW registration, emerging→established promotions, contradiction cross-references), auto-drafts up to three suggested new articles, and flags only judgement calls (out-of-scope RAW, output promotion candidates, stale rewrites). Use whenever the user says \"run a health check\", \"audit the [name] KB\", \"audit my knowledge base\", \"check the wiki\", \"action the latest health check\", or whenever a scheduled task fires it. Operates on one KB per invocation — multi-KB runs come from the scheduled task, which spawns one sub-agent per KB."
---

# Knowledge base health check

Audits one knowledge base. Auto-fixes routine drift, auto-drafts the strongest new article candidates, flags only judgement calls. Same procedure on every run, on-demand or scheduled.

## When to invoke

- The user says "run a health check", "audit the [name] KB", "audit my knowledge base", "check the wiki".
- The user says "action the latest health check" — read the latest CHANGELOG entry's pending bucket and walk through it using numbered options in chat (see "Summary format" below for the option-list pattern).
- A scheduled task fires it (the task should pass the KB path and the audit type as arguments).

If the user doesn't specify which KB and more than one exists, ask.

## Procedure

1. **Read state files.** Open `<KB>/CLAUDE.md`, `<KB>/CHANGELOG.md` (most recent block at top), `<KB>/Wiki/QUESTIONS.md`, `<KB>/RAW/_INGESTED.md`. The KB's CLAUDE.md tells you the focus areas, the article standard, and the path to the user's writing rules.

2. **Skip-if-no-changes.** If the top CHANGELOG entry is itself a health check (no compile pass, manual edit, or new output since), write `## YYYY-MM-DD — Health check skipped (no changes since last check)` to the top of CHANGELOG and stop.

3. **Find the writing rules.** Look in the KB's CLAUDE.md (and the top-level `Knowledge Base/CLAUDE.md`) for a path to a writing-rules file. Load the banned words and spelling rules. If no writing rules are referenced, skip the writing-rules audit.

4. **Pick audit scope.** Full audit on the 1st of January, April, July, October — read every article in `<KB>/Wiki/`. Delta audit otherwise — read every article modified since the last health check, plus three random ones for sample coverage.

5. **Read RAW frontmatter** for any files added since the last check (use `<KB>/RAW/_INGESTED.md` to find the cut-off).

6. **Read all outputs** in `<KB>/Outputs/` filed since the last health check.

7. **Run the audit and apply the fixes** described below. Track everything as you go for the CHANGELOG entry.

8. **Write the CHANGELOG entry.** Append at the top of `<KB>/CHANGELOG.md` using the format below.

9. **Print the summary** in the format below. If interactive and pending items exist, follow the summary with a numbered list of pending categories (see "Summary format" below) and wait for the user to pick which to walk through.

## Auto-fixes (applied in place)

- **Writing rules.** Banned-word swaps where the replacement is unambiguous. US → UK spellings via dictionary. Title-case headings → sentence case. Em-dash bullet pattern (`**Term** —`) replaced with `**Term:**` aggressively across all articles — colons are the right punctuation for label-and-description bullets.
- **Backlinks.** Repoint broken `[[link]]`s where the wiki article exists with that exact slug.
- **RAW registration.** Add orphan files in `<KB>/RAW/` to `_INGESTED.md` if they have valid frontmatter (title, author, source_url, date_added, date_published, type) and the topic fits the KB's Focus Areas.
- **Promotions.** Articles marked `Status: emerging` that now have two or more independent supporting sources → `Status: established`.
- **Contradictions are embraced, not reconciled.** When two articles disagree on the same question, do not try to harmonise them. Add a `**Counterpoint:** [[other-article]] argues...` line to each article so the reader sees both positions. Log the tension under a "Held tensions" heading in `QUESTIONS.md` (do not duplicate existing entries). Two well-sourced articles disagreeing is a feature of a good knowledge base, not a defect.
- **Gap mirroring.** New gaps surfaced during the audit go into `QUESTIONS.md` (do not duplicate existing entries).

## Auto-drafted new articles

Surface candidates from three streams: orphan references mentioned in three or more articles, outputs whose synthesis isn't anywhere in the wiki yet, held tensions that warrant their own treatment. Rank by strength of existing support. Draft up to three per run.

For each candidate:

1. If existing RAW already covers it, draft directly from RAW.
2. If existing RAW is thin, web search for primary sources. Save each new source to `<KB>/RAW/` verbatim with the full required frontmatter. Register in `_INGESTED.md` before citing.
3. Draft the article using the standard wiki article structure described in the KB's CLAUDE.md (typically: Status, Last updated, Sources, Summary, Body, Related, Open Questions). Default Status to `emerging`. Apply the writing rules.
4. Add inbound links from any existing article that mentions the topic.
5. Add the new article to `<KB>/Wiki/INDEX.md`.

If a candidate has insufficient existing RAW or no surfaceable web evidence to support an honest article, don't fabricate — log it as `Article candidate held: <name> (insufficient evidence)` and skip. Better to surface a gap than to write thin filler.

## Flag-only (pending judgement)

Log these in the pending bucket of the CHANGELOG entry. Do not auto-action.

- **RAW that looks out of scope or malformed.** The librarian doesn't relocate or remove RAW without the user's say-so.
- **Outputs that look like wiki promotion candidates.** The lift / merge / split call is a creative judgement.
- **Stale articles where new RAW exists** but a rewrite would substantially change voice or structure.
- **Banned-word violations where the right replacement isn't obvious.**

## CHANGELOG entry format

Append at the top of `<KB>/CHANGELOG.md`:

```
## YYYY-MM-DD — Health check (delta) | (full quarterly)

Audit: N of M articles read. Outputs reviewed: N. RAW added since last check: N.

Auto-fixed:
- writing-rules: N fixes across N files
- backlinks: N broken links repointed
- RAW: registered <filenames>
- em-dash bullets: N converted
- promotions: <slug> emerging → established
- contradictions: cross-references added in <article-a> ↔ <article-b>
- new articles drafted: <slug-a>, <slug-b>, <slug-c>

Pending judgement:
- <category>: <one-line description>
```

If there's nothing in either bucket, replace both with a single line: `Clean — no fixes, no pending items.`

## Summary format

Print after the run:

```
Health check — <KB name>, <YYYY-MM-DD>

- Audit: <delta|full>, N articles read
- Auto-fixed: N items | new articles drafted: N | candidates held: N
- Pending judgement: N
- See CHANGELOG: <path>
```

If running interactively and pending items exist, follow the summary with a numbered list of pending categories in chat:

```
Which pending items would you like to walk through?
1. <category-a> (N items)
2. <category-b> (N items)
3. <category-c> (N items)
N+1. None — close out the review
```

Wait for the user's reply (number or label), then walk through that category. Do not use `AskUserQuestion` — plain numbered options work reliably across CoWork and Claude Code.

## Boundaries

- **One KB per invocation.** The scheduled task runs the skill once per active KB, each in its own sub-agent so token use stays predictable as the system scales to many KBs.
- **Does not delete from RAW.** Out-of-scope flags surface for the user.
- **Does not modify CLAUDE.md or writing-rules files.** Those belong to the user.
- **Does not fabricate content** when evidence is thin. Logs the candidate as held instead.
- **Direct quotes are preserved verbatim** even if they contain banned words or non-British spellings.
- **Navigation files are exempt from writing-rules audits**: `INDEX.md`, `CHANGELOG.md`, `QUESTIONS.md`, `_INGESTED.md`, `CLAUDE.md`.

## Setting up the monthly scheduled task

This section is for first-time setup, not for runtime. The librarian reads it once when the user creates their first KB and accepts the offer to schedule monthly health checks. After the task exists, this section is irrelevant — the task self-discovers active KBs each run, so adding more KBs later doesn't require a new task.

Use Cowork's built-in `schedule` skill with the values below.

**Task ID:** `knowledge-base-monthly-health-check`

**Description:** `Monthly KB health check across every active knowledge base. Spawns one sub-agent per KB so token use stays predictable as the system scales. Posts the summary as a chat completion comment — no external pushes.`

**Schedule (cron, local time):** `0 9 1 * *` — 9am on the 1st of every month.

**Notify on completion:** yes.

**Prompt** (paste into the schedule skill; replace `<KB-root>` with the absolute path to the user's KB system root folder before submitting — for example `/Users/<you>/Claude CoWork/KNOWLEDGE/`. Replace `<COWORK-root>` with the absolute path to the CoWork folder itself — for example `/Users/<you>/Claude CoWork`):

```
Your working folder is `<COWORK-root>/`. Before doing anything else, read `<COWORK-root>/KNOWLEDGE/CLAUDE.md` so you understand how the KB system works. Scheduled tasks open in a fresh Claude session without folder context, so this re-anchoring step matters.

Then run the monthly knowledge base health check across every active KB.

Steps:

1. List active KBs. Find every immediate subfolder under `<KB-root>/` that contains a `CLAUDE.md` file. Each is one active KB. Build a list of full paths.

2. Pick the audit type for this run.
   - If today's date is the 1st of January, April, July, or October → audit type is `full` (quarterly full audit).
   - Otherwise → audit type is `delta` (read articles modified since the last health check plus three random others).

3. Spawn one general-purpose sub-agent per active KB, in parallel. Send all sub-agent calls in a single message so they run concurrently. Each sub-agent gets its own context window, which keeps token use predictable as the number of KBs grows. Each sub-agent's prompt:

   > Run the `knowledge-base-health-check-skill` skill against the knowledge base at `<full path>`. Audit type: `<delta|full>`. This is an unattended scheduled run — apply all auto-fixes, auto-draft up to three new articles where there's enough evidence, log everything to the KB's CHANGELOG. Do not pause for user input.
   >
   > When done, return exactly one line in this format:
   > `KB: <name> | type: <delta|full> | articles read: N | auto-fixed: N | new articles: N | candidates held: N | pending: N | clean: <yes|no>`
   >
   > If the skip-if-no-changes precondition fires, return:
   > `KB: <name> | skipped (no changes since last check)`

4. Collect the one-line summaries from each sub-agent.

5. Print one completion comment in chat. This is the only report — there's no external push. Format:

   ```
   **Monthly KB health check ran.**

   <total> KB(s) audited, <skipped> skipped (no changes since last check). Auto-fixed <total auto-fixed> items across all KBs. Drafted <total new articles> new articles. <P> KB(s) have pending judgement items.

   Per-KB:
   - <each sub-agent's one-line summary as a bullet>

   CHANGELOGs:
   - <path to each KB's CHANGELOG.md>

   Say "action the latest health check" to walk through any pending items.
   ```
```

Everything else in the prompt stays as written.
