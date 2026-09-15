# Migrating supram-oss state to the paid product

> **Ownership:** the migration is implemented in the **Supram CLI**, not in this repository.
> `supram-oss` ships no migration code. This document only describes the state boundary so the
> protocol and the runtime stay compatible.

This document answers a specific question: **when a project that uses `supram-oss` upgrades to
the paid Supram engine, what happens to its state?**

Short answer: the state directory is converted from `.supram-oss/` to `.supram/`. The protocol
you already follow does not change. The only thing that changes is who executes and enforces it.

---

## 1. Why the directory changes

| Phase | State directory | Manifest / handover | Executes / enforces |
|---|---|---|---|
| Protocol only | `.supram-oss/` | `.supram-oss/manifest.json`, `.supram-oss/handover.yaml` | the AI agent, following `AGENTS.md` + `commands/` |
| Paid product | `.supram/` | `.supram/manifest.json`, `.supram/handover.yaml` | the Supram engine (licensed, non-forkable) |

The separate name makes the runtime boundary explicit: `.supram/` means a licensed engine is
present. It also lets the engine detect and convert existing `.supram-oss/` state without
ambiguity, and prevents "is this project on the free or paid tier?" guesswork.

The **protocol artifacts are byte-for-byte the same shape** in both. This is deliberate: the
migration must not invalidate a project's constitution, BRD, architecture, or in-flight slices.

---

## 2. What migrates (project-owned state is never lost)

Preserved as-is:

- `.supram-oss/CONSTITUTION.md`
- `.supram-oss/BRD.md`
- `.supram-oss/ARCHITECTURE.md`
- `.supram-oss/design-registry.yaml`
- `.supram-oss/SLICE_LOG.md`
- `.supram-oss/phases/active/**` and `.supram-oss/phases/archive/**`
- `.supram-oss/specs/active/**` and `.supram-oss/specs/done/**`
- `.supram-oss/technology.yaml`
- project-owned `sanity` / integration test region

Added by the engine:

- `.supram/manifest.json` (generation, schema, release)
- `.supram/memory/` (typed project graph)
- `.supram/handover.yaml` (derived snapshot)
- enforcement, evidence, and dashboard runtime

---

## 3. Migration procedure (implemented in `supram-cli`)

`supram-oss` itself never performs this migration. The Supram CLI (`supram-cli`) does, in this order:

1. **Preflight** — verify the project is clean (`git status`), record branch/revision, and
   create a backup of `.supram-oss/`.
2. **Detect** — read the `.supram-oss/` layout and manifest to determine the source
   schema generation.
3. **Plan** — report exactly which files will be created, updated, moved, or preserved. The
   plan must not overwrite project-owned artifacts.
4. **Consent** — stop and require explicit human approval before any write.
5. **Apply** — create `.supram/`, move protocol state across, write the manifest, and build
   the project graph from existing artifacts.
6. **Validate** — confirm constitution, BRD, architecture, slice log, design registry, and all
   active/archived work are intact; confirm the manifest and target release.
7. **Finalize** — regenerate the handover snapshot, record the migration in `SLICE_LOG.md`,
   and remove the now-empty `.supram-oss/` directory (or leave a pointer file, per project
   preference).

In-flight slices are **not** migrated mid-flight. They keep their existing workflow level unless
the human separately approves a mid-slice migration.

---

## 4. What the user experiences

1. Install the paid Supram engine (see the product install guide).
2. Run `/supram:upgrade`.
3. Approve the migration plan at the gate.
4. Continue exactly where you left off — same branches, same specs, same gates.

No re-initialization. No re-writing of documents. No workflow re-learning.

---

## 5. Rollback

Before applying, the engine creates a timestamped backup of `.supram-oss/` and records the
migration plan. If validation fails, restore the backup, delete `.supram/`, and the project is
back to protocol-only state. The `.gitignore` in this repository ignores both `.supram-oss/`
and `.supram/`, so neither appears as an untracked surprise in the protocol repo.

---

## 6. FAQ

**Do I have to migrate?** No. `.supram-oss/` continues to work as a protocol with an AI agent
driving it. Migration only happens when you adopt the engine.

**Can I use the paid engine on a project that never used supram-oss?** Yes — the engine can
initialize `.supram/` directly.

**Does the protocol change after migration?** No. Commands, gates, and `/supram:` names are stable
by design. If a paid-only command appears, it is additive, not a replacement.

**Where is the migration implemented?** In the Supram CLI (`supram-cli`). Track and evolve the
migration contract there; keep this document in sync as the boundary reference.
