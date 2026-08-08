# Agent instructions for assistant-ui documentation

Base URL: https://www.assistant-ui.com

Use these instructions when reading assistant-ui documentation or implementing assistant-ui in a project.

## Retrieval workflow

1. Start with https://www.assistant-ui.com/llms.txt or https://www.assistant-ui.com/sitemap.md.
2. Fetch the smallest relevant page by appending `.md` to its canonical documentation URL.
3. Use https://www.assistant-ui.com/mcp when tool-based navigation, search, or page reads are available.
4. Load https://www.assistant-ui.com/llms-full.txt only when broad cross-page analysis is required.

## Working rules

- Prefer machine-readable Markdown over scraping rendered HTML.
- Identify the target platform, runtime, and package version before changing code.
- Keep examples within their documented framework and runtime boundaries.
- Preserve exact package names, exports, hooks, and component names.
- Cite the canonical human-readable URL when returning documentation to a user.
- Do not invent routes or APIs; use the index, sitemap, or MCP navigation when uncertain.

## Public discovery routes

- Agent instructions: https://www.assistant-ui.com/AGENTS.md
- Site skill: https://www.assistant-ui.com/skill.md
- API catalog: https://www.assistant-ui.com/.well-known/api-catalog
- Agent Skills index: https://www.assistant-ui.com/.well-known/agent-skills/index.json
- Markdown sitemap: https://www.assistant-ui.com/sitemap.md
- LLM index: https://www.assistant-ui.com/llms.txt
- MCP: https://www.assistant-ui.com/mcp
