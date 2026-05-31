# Contributing to cari-tools

Thanks for your interest! `cari-tools` gets better the more authoritative **sources** and workflow refinements it knows about.

## Ways to contribute

- **Add a source.** Know a good registry or curated list for MCP servers, plugins, or skills? Add it to the relevant "Search live per category" section in `plugins/cari-tools/skills/cari-tools/SKILL.md`.
- **Improve the workflow.** Better de-dupe checks, clearer ranking criteria, more accurate install-command syntax.
- **Fix bugs.** Wrong install command, broken source URL, stale guidance.
- **Suggest, don't bloat.** Quality over quantity — the skill recommends 3–6 tools per category on purpose.

## Project layout

```
.claude-plugin/marketplace.json          # marketplace catalog
plugins/cari-tools/
├── .claude-plugin/plugin.json           # plugin manifest
└── skills/cari-tools/SKILL.md           # the skill itself — most edits go here
```

The skill is plain Markdown with YAML frontmatter. There is no build step.

## Test your change locally

```bash
# Load the plugin into a Claude Code session without installing it
claude --plugin-dir ./plugins/cari-tools

# Validate structure (if your Claude Code version supports it)
claude plugin validate ./plugins/cari-tools
```

Then run `/cari-tools:cari-tools` and confirm your change behaves as expected.

> **CI note:** the validation workflow lives at `ci/validate.yml`. To make it run on
> GitHub, a maintainer must copy it to `.github/workflows/validate.yml` and push with a
> token that has the `workflow` scope (or add it via the GitHub web UI).

## Pull request flow

1. Fork and create a branch (`git checkout -b add-foo-source`).
2. Make your change. Keep `SKILL.md` frontmatter (`name`, `description`) intact.
3. Run the validation above; make sure CI passes.
4. Open a PR using the template. Explain *why* the change helps users find better tools.

## Frontmatter rules

- `name` must stay `cari-tools`.
- `description` must clearly state when Claude should invoke the skill (it's how the model decides to use it).

By contributing you agree your work is licensed under the project's [MIT License](LICENSE).
