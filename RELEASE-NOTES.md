# Release v0.1.0

**Date**: 2026-09-15

## What's New

- Initial public release of **Supram OSS** — the file-based software engineering protocol
  for AI coding assistants.
- Protocol reference only: conventions, templates, command contracts, and persona definitions.
- Runtime capabilities (enforcement, evidence, dashboards, memory/migrations, skill
  installation) are provided by the Supram engine (paid offering), not by this repository.
- Added `MIGRATION.md` describing the upgrade of `.supram-oss/` project state to `.supram/`
  when adopting the paid product.
- Unified slice contract: `/supram:define` produces a single `spec.yaml` (design and tasks
  internalised). Two human gates: `/supram:approve` (before build) and `/supram:release` (after
  verify). `/supram:review-pre-verify` is an L-level-only agent review, not a human gate.

## Out of scope in this repository

This repository ships the protocol only. The following capabilities are intentionally absent
from the OSS repository and are provided by the Supram engine (paid offering):

- enforcement, evidence capture, traceability, and blocked-state detection
- status dashboard and status server
- migration engine
- skill selection/installation
- release automation and pre-commit hooks
- runtime-specific templates
- legacy standalone contracts and their artifacts: `/supram:design`, `/supram:tasks`,
  `/supram:review-pre-build`, the Sr Architect persona, and the standalone `design.md`/
  `tasks.md`/`spec.md` templates (superseded by `spec.yaml`)

## License

AGPL-3.0-only. See [LICENSE](LICENSE).
