---
name: mcp-builder
description: Guide for creating high-quality MCP (Model Context Protocol) servers that enable LLMs to interact with external services through well-designed tools. Use when building MCP servers to integrate external APIs or services, whether in Python (FastMCP) or Node/TypeScript (MCP SDK).
---

# MCP Server Development Guide

## Overview

Create MCP servers that enable LLMs to interact with external services through well-designed tools.
Quality is measured by how well the server enables LLMs to accomplish real-world tasks.

---

## Four-Phase Workflow

### Phase 1 — Research and Planning

**MCP Design principles:**
- Balance API endpoint coverage with specialised workflow tools
- Use consistent prefixes: `github_create_issue`, `github_list_repos`
- Design tools that return focused, relevant data
- Error messages must be actionable — guide agents toward solutions

**Recommended stack:**
- Language: TypeScript (broad usage, static typing, good AI generation support)
- Transport: Streamable HTTP (remote, stateless JSON) or stdio (local)

**Load framework docs:**
- TypeScript SDK: `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
- Python SDK: `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`

---

### Phase 2 — Implementation

**Core infrastructure:**
- API client with authentication
- Error handling helpers
- Response formatting (JSON / Markdown)
- Pagination support

**Per tool:**
- Input schema: Zod (TypeScript) or Pydantic (Python) with constraints and examples
- Output schema: define `outputSchema` where possible
- Async/await for I/O
- Actionable error messages
- Annotations: `readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`

---

### Phase 3 — Review and Test

**Code quality checklist:**
- No duplicated code (DRY)
- Consistent error handling
- Full type coverage
- Clear tool descriptions

**TypeScript:** `npm run build` then `npx @modelcontextprotocol/inspector`
**Python:** `python -m py_compile your_server.py` then MCP Inspector

---

### Phase 4 — Evaluations

Create 10 evaluation questions. Each must be:
- Independent, read-only, complex, realistic, verifiable, stable

XML output format:
```xml
<evaluation>
  <qa_pair>
    <question>Your question here</question>
    <answer>The answer</answer>
  </qa_pair>
</evaluation>
```

---

## Reference

- MCP spec: `https://modelcontextprotocol.io/sitemap.xml`
- TypeScript SDK: `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
- Python SDK: `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
