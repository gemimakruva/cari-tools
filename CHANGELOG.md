# Changelog

All notable changes to this project are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-05-31

### Fixed
- `SKILL.md` frontmatter now uses a folded block scalar (`>-`) for `description`.
  The previous inline form contained `Triggers:` and embedded quotes, which made
  the YAML ambiguous; `claude plugin validate` failed to parse it and dropped all
  metadata at runtime. Now validates cleanly.

## [1.0.0] - 2026-05-31

### Added
- Initial release of the `cari-tools` skill, packaged as a Claude Code plugin + marketplace.
- Live search across official and community sources for MCP servers, plugins, and skills.
- De-duplication against already-installed tools.
- Ranked recommendations with exact install commands and an opt-in install step.
- Two install methods: plugin marketplace and standalone skill.

[1.0.1]: https://github.com/gemimakruva/cari-tools/releases/tag/v1.0.1
[1.0.0]: https://github.com/gemimakruva/cari-tools/releases/tag/v1.0.0
