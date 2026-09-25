# Spec Index — `supram-oss`

**Scope owned here:** the portable, open-source-facing contracts — notably the **canonical agents
template** that any agent/client can adopt, independent of the CLI.

## Normative specs (owned here)
| spec | purpose | status |
|---|---|---|
| `CANONICAL-AGENTS-SPEC.md` (portable protocol/template) | agent contract + dual-mode fallback, client-agnostic | **owned (moved)** |
| OSS standard (`.supram/` format, licensing) | open standard | owned here |

## Imported cross-repo contracts (from `supram-contracts`)
| contract | used by |
|---|---|
| retrieval envelopes | agent tooling |
| lifecycle/claim schemas | governance tooling |

## Intent / ADRs (from `supram-ideation`)
- `SPEC-OWNERSHIP-INDEX.md`; canonical-agents philosophy.

> The CLI-side **installation/scaffold/enforcement** of this template lives in `supram-cli`
> (`init` → `AGENTS.md`), not here.

## Migration
Status: **pending** — see `supram-ideation/SPEC-OWNERSHIP-INDEX.md`.
