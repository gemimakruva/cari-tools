# cari-tools

> A **tool scout** for [Claude Code](https://code.claude.com). Tell it what you're working on; it searches the live internet — official registries, GitHub, and the web — and recommends the **Skills, Plugins, and MCP servers** worth installing, with the exact install commands. It also checks what you already have so it never recommends a duplicate.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-blueviolet)](https://code.claude.com/docs/en/plugins)

*"Cari" is Indonesian for "search / find."*

---

## What it does

Most people don't know which of the thousands of community MCP servers, plugins, and skills actually fit their workflow. `cari-tools` closes that gap. Given a need like *"I do market research and analyze client documents"*, it will:

1. **Understand the need** (asks a quick clarifying question only if necessary).
2. **De-duplicate** against what you already have (`claude mcp list`, `~/.claude/skills/`, your installed marketplaces).
3. **Search live** across authoritative sources for MCP servers, plugins, and skills.
4. **Rank** the best 3–6 per category in a table — what it does, why it fits, official vs community.
5. **Give exact install commands** and offer to run them after you confirm.

It uses Claude's `WebSearch` / `WebFetch` tools for **live** results, and verifies every candidate against its real repo before recommending it.

## Install

### Option A — as a plugin (recommended)

```shell
/plugin marketplace add gemimakruva/cari-tools
/plugin install cari-tools@cari-tools
```

Then invoke it (plugin skills are namespaced):

```shell
/cari-tools:cari-tools find an MCP and skill for market research and client document analysis
```

### Option B — as a standalone skill

```bash
git clone https://github.com/gemimakruva/cari-tools /tmp/cari-tools
cp -r /tmp/cari-tools/plugins/cari-tools/skills/cari-tools ~/.claude/skills/
```

Restart Claude Code (or run `/reload-skills`), then invoke it with the clean name:

```shell
/cari-tools find a tool for <your need>
```

## Example

> **You:** find an MCP & skill for market research and client document analysis

> **cari-tools:** *(after checking what you already have)*
>
> | Tool | Category | What it does | Why it fits | Source | Key? |
> |------|----------|--------------|-------------|--------|------|
> | MarkItDown | MCP | PDF/DOCX/XLSX → clean markdown | Read client docs locally, no data leaves your machine | Microsoft (official) | No |
> | Tavily | MCP | Real-time web search + extraction with citations | Citation-heavy market research | community | Yes |
> | xlsx | Skill | Read & build Excel natively | Financial models & data tables | anthropics/skills | — |
>
> ```bash
> claude mcp add markitdown -- uvx markitdown-mcp
> claude mcp add tavily -e TAVILY_API_KEY=xxx -- npx -y tavily-mcp@latest
> ```

## Sources it searches

- **MCP:** [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io), [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers), [modelcontextprotocol.io](https://modelcontextprotocol.io), [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)
- **Plugins:** the official `claude-plugins-official` and community [`anthropics/claude-plugins-community`](https://github.com/anthropics/claude-plugins-community) marketplaces, plus the web
- **Skills:** [anthropics/skills](https://github.com/anthropics/skills) and the wider community

## Security

`cari-tools` only **reads public data** while searching — that part is safe. It never auto-installs anything without your confirmation. Community (third-party) tools are clearly flagged, and you should review their source before installing, since installing an MCP server or plugin runs third-party code on your machine.

## Contributing

PRs welcome — especially new authoritative **sources** to search and improvements to the recommendation workflow. See [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md).

## License

[MIT](LICENSE) © gemimakruva
