---
name: video-import-public
description: >-
  First step for getting a video INTO Descript straight from Claude — no Google Drive
  sync, no dragging into the desktop app, no file-size limit. Given a local video (a
  path, or "the latest in my Downloads") or a hosted URL, it uploads the file directly
  into a new Descript project, confirms it transcribed, and hands off to
  content-gold-miner-public or script-prep-public. TRIGGER on "import this video," "upload this
  to Descript," "get this into Descript," "upload my footage," "put this clip in
  Descript," or any variation of getting a video into Descript before mining or editing.
  Does NOT mine, cut, or caption. NOTE: the local-file upload needs a terminal + local
  files (Claude Code or Cowork); on claude.ai Chat, use the URL-import path instead.
---

# Video Import (Public)

Get a video into Descript straight from Claude — any size — then hand off. (Public version.) Front of the pipeline: **video-import-public → content-gold-miner-public / script-prep-public → your editing step in Descript.**

## What this skill does (and the boundary)

It solves the footage-in problem: take a video that's on your computer (or at a URL) and get it into a Descript project, ready to mine or prep — without Google Drive sync or dragging into the desktop app, and with NO ~1.5 GB size ceiling.

It stops at "the video is in Descript and fully transcribed; here's the project." It does NOT mine for ideas (`content-gold-miner-public`), assemble a script (`script-prep-public`), or cut/caption.

## Where this runs (read first)

- The **local-file upload** below runs `curl` from your machine, so it needs **local filesystem + a terminal** — i.e. **Claude Code or Claude Cowork**.
- On **claude.ai Chat** there's no local terminal or access to your files, so use the **URL-import path** (below) instead — host the file at a direct/Dropbox link and import by URL.

## The method (works for any size)

The upload goes straight from your machine to Descript's storage via a LOCAL `curl`, bypassing Claude's sandbox — so there's no file-size ceiling.

1. **Identify the source video** — a path you give, or "the latest in Downloads" → `find ~/Downloads -maxdepth 1 -type f \( -iname '*.mp4' -o -iname '*.mov' -o -iname '*.m4v' \) -exec stat -f '%m %N' {} + | sort -n | tail -1`. Confirm the file before uploading. (Cloud-stub guard: if the file's size looks implausibly small for its length, it may be a non-downloaded placeholder — download it fully first.)
2. **Exact size + type.** `stat -f%z "<file>"` for the byte count (the declared size must match EXACTLY). content_type by extension: `.mp4`/`.MP4` → `video/mp4`, `.mov` → `video/quicktime`, `.m4v` → `video/x-m4v`.
3. **Request the upload URL.** `import_media` in direct-upload mode (`content_type` + `file_size`), with `project_name` and `add_compositions` (vertical 1080×1920 by default for reels; use 1920×1080 for landscape — ask if unsure). The response includes `upload_urls[key].upload_url` (presigned, valid ~3 hrs), `project_id`, `project_url`, and the import `job_id`.
4. **PUT the bytes locally.** `curl -sS -T "<file>" -H "Content-Type: application/octet-stream" -w '\nHTTP %{http_code} uploaded %{size_upload} bytes in %{time_total}s\n' "<upload_url>"`. `-T` streams the file (no memory buffering). **For multi-GB files, run the curl as a BACKGROUND command** so the foreground timeout can't kill it. Success = **HTTP 200 AND `size_upload` equals the declared `file_size`.**
5. **Confirm the import.** `wait_for_job` on the import `job_id` → `result.status` "success", `media_status` "success", real `duration_seconds`. Show the `project_url`.

## Verify completeness (did it get the whole video?)

The upload only ever sends what's in the local file — so if anything's missing, it's the SOURCE, not the transfer:
- File size didn't change since upload (re-`stat` — not still syncing).
- Container duration ≈ Descript's reported duration: `mdimport -t -d2 "<file>" 2>&1 | grep -i duration` (kMDItemDurationSeconds), or `mdls -name kMDItemDurationSeconds`.
- **If footage seems cut off:** the file genuinely ends there. Cameras often SPLIT a long recording into the next-numbered file — look for it (often still on the SD card), import that too, and stitch in Descript. If there's no continuation, it's a source cutoff (a pickup re-record), not an upload problem.

## URL import (the claude.ai path / any-size fallback)

If the video is already hosted, skip the curl and pass `url` to `import_media`. Direct links and **Dropbox** direct-download links work (probe first). **Google Drive is unreliable** — private files hit an auth page, and large link-shared files hit a "confirm download" interstitial. This is the path to use on claude.ai Chat (no local terminal).

## Hand-off

Once the video is in Descript and transcribed, point to the next step: `content-gold-miner-public` (mine a long recording for reel ideas) or `script-prep-public` (one intended piece → best takes + final script).

## Relationship to other skills

- First step in the pipeline: video-import-public → `content-gold-miner-public` / `script-prep-public` → your editing step in Descript.
- Owns ONLY the import; never mines, preps, cuts, or captions.

## Change log

- 2026-06-29: Public version — de-personalized. Generic source handling, orientation default with ask, and an explicit "where this runs" section (local upload = Code/Cowork; claude.ai Chat = URL import). The method (import_media direct-upload → local curl PUT, any size, bypassing the sandbox) and the completeness/continuation-file check are unchanged.
- 2026-07-06: Renamed skill to `video-import-public` (cross-references updated to the new public skill names).
