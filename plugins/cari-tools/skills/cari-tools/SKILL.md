---
name: cari-tools
description: Find & recommend the Skills, Plugins, and MCP servers worth installing for the user's work. Searches the live internet (official registries + GitHub + web), matches them to the stated need, de-duplicates against what's already installed, then hands back the exact install commands. Triggers: "find tools", "find a skill/plugin/mcp for X", "what should I install for X", "recommend an MCP", "/cari-tools".
---

This skill turns Claude into a "tool scout": given a work need, it searches the internet for the most relevant Skills, Plugins, and MCP servers, then returns a ranked recommendation with exact install commands. Use the WebSearch and WebFetch tools for live data — do NOT rely on the model's memory, which goes stale fast because registries and marketplaces change quickly.

> **Why "cari-tools"?** "Cari" is Indonesian for "search/find". The skill was born in an Indonesian workflow; the name stuck.

## Required steps (in order)

### 1. Understand the need
If the user hasn't stated a specific need, ask briefly with AskUserQuestion:
- Work domain (e.g. web scraping, documents/PDF, web design, databases, deploy, research, Google Workspace).
- Which categories to search: **Skill**, **Plugin**, **MCP**, or all (default: all).
If the need is already clear from the prompt, proceed — don't over-ask.

### 2. Check what's ALREADY installed (de-dupe first)
Never recommend something the user already has. Inspect:
- **Installed skills:** `ls ~/.claude/skills/` plus the skills listed in this session's system prompt.
- **Plugin marketplaces:** `ls ~/.claude/plugins/cache/` (whatever the user already added).
- **Installed MCP servers:** `claude mcp list` (or read the `mcpServers` keys in `~/.claude.json` and `~/.claude/settings.json`).
Note everything already present → drop it from the results, or mark it "already installed".

### 3. Search live per category
For each requested category, use WebSearch + WebFetch against the authoritative sources below. Always search using the user's need as keywords.

**MCP servers:**
- Official registry: `https://registry.modelcontextprotocol.io`
- Official repo: `https://github.com/modelcontextprotocol/servers` (the `src/` folder = official reference servers).
- Docs: `https://modelcontextprotocol.io`
- Community list: `https://github.com/punkpeye/awesome-mcp-servers` and a WebSearch for `"MCP server <need>"`.

**Plugins (Claude Code):**
- Anthropic marketplaces: official (`claude-plugins-official`, available everywhere) and community (`anthropics/claude-plugins-community`, add via `/plugin marketplace add anthropics/claude-plugins-community`).
- WebSearch `"claude code plugin <need>"`, `site:github.com claude-code plugin marketplace`.
- Also inspect any marketplaces the user already added under `~/.claude/plugins/cache/`.

**Skills (Agent Skills):**
- Official Anthropic repo: `https://github.com/anthropics/skills`.
- WebSearch `"claude agent skill <need>"`, `"awesome claude skills"`.
- Compare against what's already installed before suggesting anything similar.

Verify each candidate with WebFetch to its repo/README before recommending it — never cite a tool you can't confirm exists. Clearly mark which are official (Anthropic / modelcontextprotocol) vs third-party community, and remind the user to review community tool source before installing (supply-chain risk).

### 4. Present a ranked recommendation
Show a compact table, most relevant first. Max 3–6 per category (quality > quantity):

| Tool | Category | What it does | Why it fits | Source (official/community) | Install command |
|------|----------|--------------|-------------|-----------------------------|-----------------|

### 5. Give the CORRECT install commands
Be exact, not approximate:

- **MCP (stdio/local):** `claude mcp add <name> -- <command> <args>`
  e.g. `claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/projects`
- **MCP (HTTP/SSE remote):** `claude mcp add --transport http <name> <url>`
- **MCP (with API key):** `claude mcp add <name> -e API_KEY=xxx -- npx -y <package>`
- **MCP (from JSON):** `claude mcp add-json <name> '<json>'`
- **Plugin:** `/plugin marketplace add <owner/repo>` then `/plugin install <name>@<marketplace>`
- **Skill:** clone/copy the skill directory into `~/.claude/skills/<name>/` (must contain a `SKILL.md`).
  Note: a standalone `.md` file directly in `skills/` is NOT recognized — it must be a directory with `SKILL.md` inside.

State the scope: MCP & plugins are usually global (all projects); a skill can be global (`~/.claude/skills/`) or per-project (`<project>/.claude/skills/`).

### 6. Offer to install (confirm first)
Installing is consequential (it changes global config and runs third-party code). Default: recommend first, then ask "which ones should I install?". Install ONLY after the user picks. After installing an MCP, remind them some need an API key / auth → point them to set the env var or run the auth flow. A `claude` session restart may be needed before new tools are picked up.

## Limits
- Searching only reads public data — safe.
- Never auto-install without user confirmation.
- For community tools, always flag the risk and suggest reviewing the source first.
