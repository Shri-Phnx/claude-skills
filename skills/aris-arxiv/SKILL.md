---
name: arxiv
description: Search, download, and summarize academic papers from arXiv. Use when user says "search arxiv", "download paper", "fetch arxiv", "arxiv search", "get paper pdf", or wants to find and save papers from arXiv to the local paper library.
argument-hint: [query-or-arxiv-id]
allowed-tools: Bash(*), Read, Write
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/arxiv
---

# arXiv Paper Search & Download

Search topic or arXiv paper ID: $ARGUMENTS

## Constants

- **PAPER_DIR** - Local directory to save downloaded PDFs. Default: `papers/`
- **MAX_RESULTS = 10**

> Overrides:
> - `/arxiv "attention mechanism" - max: 20`
> - `/arxiv "2301.07041" - download`
> - `/arxiv "query" - download: all`

## Workflow

### Step 1: Parse Arguments
Parse for: Query or ID, `- max: N`, `- dir: PATH`, `- download`, `- download: all`

### Step 2: Search arXiv
Use arXiv API (`http://export.arxiv.org/api/query`). Present results as table:
```
| # | arXiv ID | Title | Authors | Date | Category |
```

### Step 3: Download PDFs
```bash
mkdir -p PAPER_DIR && python3 -c "
import pathlib, urllib.request
out = pathlib.Path('PAPER_DIR/ARXIV_ID.pdf')
req = urllib.request.Request('https://arxiv.org/pdf/ARXIV_ID.pdf', headers={'User-Agent': 'arxiv-skill/1.0'})
with urllib.request.urlopen(req, timeout=60) as r:
    out.write_bytes(r.read())
"
```

### Step 4: Update Research Wiki (if active)
If `research-wiki/` exists: `python3 research_wiki.py ingest_paper research-wiki/ --arxiv-id "<arxiv_id>"`

## Key Rules
- Always show arXiv ID prominently
- Verify downloaded PDFs: file must be > 10 KB
- Rate limit: wait 1 second between consecutive PDF downloads
- Never overwrite an existing PDF

## Follow-up:
```
/research-lit "topic"     - multi-source review
/novelty-check "idea"     - verify novelty
```
