---
name: semantic-scholar
description: Search published venue papers (IEEE, ACM, Springer, etc.) via Semantic Scholar API. Complements /arxiv (preprints) with citation counts, venue metadata, and TLDR. Use when user says "search semantic scholar", "find IEEE papers", "find journal papers", or wants published literature beyond arXiv preprints.
argument-hint: query-or-paper-id
allowed-tools: Bash(*), Read, Write
source: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep/tree/main/skills/semantic-scholar
---

# Semantic Scholar Paper Search

Search topic or paper ID: $ARGUMENTS

## Role

| Skill | Source | Best for |
|-------|--------|----------|
| `/arxiv` | arXiv API | Latest preprints |
| `/semantic-scholar` | S2 API | Published papers (IEEE, ACM, Springer) with citation counts |

## Constants
- **MAX_RESULTS = 10**
- **DEFAULT_FILTERS**: `--fields-of-study "Computer Science,Engineering"`, `--publication-types JournalArticle,Conference`

## Workflow

### Step 1: Parse Arguments
Query or ID (DOI, S2 ID, ArXiv: prefix, CorpusId:), `- max: N`, `- type: journal|conference|all`, `- min-citations: N`, `- year: RANGE`

### Step 2: Search via Semantic Scholar API
`https://api.semanticscholar.org/graph/v1/paper/search`

Recommended combos:
| Goal | Flags |
|------|-------|
| High-quality journal papers | `--publication-types JournalArticle --min-citations 10` |
| CS/EE papers, recent | `--fields-of-study "Computer Science,Engineering" --year "2022-"` |
| Foundational | search-bulk `--sort citationCount:desc` |

### Step 3: Present Results
```
| # | Title | Venue | Year | Citations | Authors | Type |
```
For each: DOI link, Open Access PDF, TLDR, Also on arXiv flag.

### Step 4: Update Research Wiki (if active)

## Key Rules
- Default to filtered search — apply `--fields-of-study` and `--publication-types` unless `- fields: all`
- Citation count is gold — always show prominently
- Rate limiting: ~1 req/s without API key; set `SEMANTIC_SCHOLAR_API_KEY` for higher limits

## Follow-up:
```
/arxiv "topic"           - search arXiv preprints
/novelty-check "idea"    - verify novelty
```
