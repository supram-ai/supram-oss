# Release v0.1.0

**Date**: 2026-09-15

## What's New

- Initial public release of **Supram OSS** — the file-based software engineering protocol
  derived from `harness-eng`.
- Protocol reference only: conventions, templates, command contracts, and persona definitions.
- Runtime capabilities (enforcement, evidence, dashboards, memory/migrations, skill
  installation) are provided by the Supram engine (paid offering), not by this repository.
- Added `MIGRATION.md` describing migration of `.supram-oss/` project state to `.supram/`
  when adopting the paid product.

## Removed from upstream `harness-eng`

The following were intentionally not carried into the OSS protocol, because a forkable
repository cannot enforce conversion walls:

- `scripts/` — prerequisite enforcement, evidence sensors, traceability, blocked-state
- status dashboard and status server
- migration engine and migrations
- skill selection/installation
- release automation and pre-commit hook
- runtime-specific templates (`init-layout.json`, `migration-consent.yaml`)

## License

AGPL-3.0-only. See [LICENSE](LICENSE).
