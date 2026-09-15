# supram-oss

**A full-lifecycle software engineering protocol for AI coding assistants.**

One protocol. Any agent. Any project. From big picture to shipped feature — with human gates,
structured templates, and explicit phase transitions for large BRDs.

> This repository is the **protocol reference**: conventions, templates, command contracts, and
> personas. The **runtime** that executes and enforces the workflow — prerequisite checks,
> evidence capture, dashboards, memory/migrations, and skill installation — is the **Supram
> engine** (paid offering).

---

## Quickstart

Open your AI agent in your project directory and paste this:

```
read https://github.com/AvonS/supram-oss/blob/main/commands/init.md and follow the instructions to initialize supram-oss in this folder
```

| Folder state | What the agent does |
|--------------|---------------------|
| **Empty** | Asks questions, builds requirements from conversation |
| **Existing code** | Reverse-engineers BRD, constitution, architecture |
| **PRD + ADR ready** | Derives all documents from existing docs |
| **Has .supram-oss/** | Refreshes the protocol reference, preserves customizations |

---

## Protocol vs runtime

| Capability | This repo (supram-oss) | Supram engine (paid) |
|---|---|---|
| Workflow, templates, gates, personas | Yes | Yes |
| Command contracts (reference) | Yes | Yes |
| Deterministic gate enforcement | — | Yes |
| Evidence capture and verification | — | Yes |
| Status dashboard and approvals | — | Yes |
| Memory, project graph, migrations | — | Yes |
| Skill selection and installation | — | Yes |
| Multi-project / team workflows | — | Yes |

The protocol is a **file-based convention**, not a tool dependency. It works whether you use
Claude, Copilot, or Cursor. The runtime is what makes it dependable.

---

## The Workflow

The core philosophy is a bounded agent loop: agents work autonomously, and human gates stop
them until a person approves.

```text
/h:init → /h:define → [Human Gate 1: /h:approve] → /h:build
        → [/h:review-pre-verify — L only] → /h:verify → [Human Gate 2: /h:release]
```

Design and tasks are internalised by `/h:define` into a single `spec.yaml`; there are no separate
design or tasks commands.

### Commands

Read the command contracts from `commands/` (installed projects: `.supram-oss/commands/`).

| Command | Logical Role | What happens | Gate |
|---------|--------------|-------------|------|
| `/h:init` | Manager | Scan docs, derive constitution/BRD/architecture, plan phases | — |
| `/h:define` | Analyst | Create `spec.yaml` from the BRD, including design and tasks | — |
| `/h:approve` | Gatekeeper | **Human Gate 1** — approve the spec/design before build | Human |
| `/h:build` | Developer | Implement against the approved Evidence Contract — one commit per task | — |
| `/h:review-pre-verify` | Sr Tech Lead | Fresh-context review of build against the contract (**L only**) | Agent review |
| `/h:verify` | Gatekeeper | Run approved evidence, check acceptance criteria, write verification | — |
| `/h:release` | Gatekeeper | **Human Gate 2** — disclose deferred, merge, archive, update status | Human |
| `/h:change` | Developer | Unified bug and CR workflow: baseline → smallest delta → verify | — |
| `/h:status` | Manager | Project state snapshot (provided by the Supram engine) | — |
| `/h:upgrade-harness` | Manager | Refresh the protocol reference; hand state to the paid product | — |

> Command names and the `/h:` prefix are shared with the Supram engine and remain stable.

---

## What Gets Created

```
your-project/
├── AGENTS.md                    ← agent workflow rules
│
└── .supram-oss/
    ├── CONSTITUTION.md          ← principles and rules (never overwritten)
    ├── BRD.md                   ← business requirements (never overwritten)
    ├── ARCHITECTURE.md          ← system design (never overwritten)
    ├── technology.yaml          ← toolchain + tech decisions (never overwritten)
    ├── SLICE_LOG.md             ← build narrative
    ├── commands/                ← workflow command contracts
    ├── agents/                  ← persona definitions
    ├── templates/               ← feature and big-picture templates
    │
    ├── phases/
    │   ├── active/<phase>/features/<feature>/
    │   └── archive/<phase>/
    └── specs/
        ├── active/              ← CRs and changes
        └── done/                ← released CRs and changes
```

---

## Gates

There are **two human gates**, plus an optional agent review for L-level projects. The Supram
engine enforces them deterministically; without the engine, the human is the enforcement point.

1. **Human Gate 1 — `/h:approve`** (before build): the human approves `Ref: APPROVED` on the
   spec/design produced by `/h:define`. Nothing may be implemented before this.
2. **Human Gate 2 — `/h:release`** (after verify): the human approves `Release Ref: APPROVED`,
   deferred items are disclosed, then the work is merged. Cannot proceed without explicit approval.

**Agent review (not a human gate) — `/h:review-pre-verify`**: a Sr Tech Lead fresh-context review
of the build against the spec. It applies to **L-level projects only**; S and M skip it.

Quality comes from:
- **Templates** — constrain LLM output with structure
- **Constitution** — principles the agent must follow
- **Evidence contract** — the minimum proof required per change, enforced by the Supram engine

---

## Git Branching

```
main                               ← protected
  ├── feature/<FID>-<slug>        ← /h:define
  └── change/CHG-NNN-<slug>       ← /h:change
```

Commit convention: `type(ID): description`

---

## Skills

Skill selection and installation are provided by the Supram engine. The protocol documents
which skills apply to which stack (Go, Python, Node/TS, SQL, Git, and more), and how to
review them before adoption.

---

## Migrating to the paid product

When you adopt the paid product, your `.supram-oss/` project state migrates to `.supram/`.
The migration is implemented by the **Supram CLI**, not by this repository; this repo only
documents the state boundary. See [MIGRATION.md](MIGRATION.md). The protocol never needs to be
re-learned; only the runtime boundary changes.

`supram-oss` is the current name of the community edition and descends from `harness-eng`; the
artifact schema, gates, and evidence contract are continuous across the rename. Engine-side
migration is deferred to the CLI's port to `supram-cli` (see §0 of MIGRATION.md).

---

## Design Philosophy

> *The protocol is a convention, not a plugin.*

Like HTTP works whether you use Chrome or curl — supram-oss works whether you use
Claude, Copilot, or Cursor. The workflow is the protocol. The folder structure is the state machine.

---

## Design Rationale

The ideas behind this protocol — human gates, evidence contracts, and structured agent
workflows — are documented in *Harness Engineering in Practice*:
<https://avons.github.io/notes/harness-eng/> (upstream article; same method, prior naming).

> A dedicated content engine (guides and an updated article) is planned for this repository.

---

## Inspired By

- **spec-kit** — Constitution/spec/plan/tasks separation
- **OpenSpec** — Iterative flow, archive pattern
- **modular/skills** — Agent Skills Standard
- **BMAD** — Structured agentic development
- **AI Manifesto** — Six principles for thinking clearly with AI

---

## License

Copyright (c) 2026 [Avon Software Labs](https://avons.github.io).

Licensed under the GNU Affero General Public License v3.0 only (`AGPL-3.0-only`). See [LICENSE](LICENSE).
