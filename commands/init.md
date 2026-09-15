---
name: supram-init
description: >
  Initialize Supram in project. Detect scenario, derive docs, create scaffold.

persona: Manager
reason: Bootstrap operation
goal: Produce rich, expressive, and highly readable foundational documents (BRD, Architecture) using the provided templates in .supram-oss/templates/.

preflight:
  - read_canonical_agents: Read canonical source AGENTS.md before scanning, clarifying, creating files, or running commands
gates:
  - check: "! .supram-oss/ exists"
    on_fail: STOP, .supram-oss already exists. Use /supram:upgrade instead.

actions:
  - scan: project for existing docs (README, PRD, ADR, code)
  - classify_scenario: [A: greenfield, B: brownfield, C: documented]
  - bind_clarification_context: Treat every requested user answer as initialization input until this command stops
  - create_scaffold:
    - create the state directory `.supram-oss/` in the project root
    - copy the protocol reference (commands/, agents/, templates/, VERSION) from this repository into .supram-oss/
    - copy `AGENTS.md` to the project root `./AGENTS.md`
    - create the initial state files (from the matching templates):
      - create .supram-oss/CONSTITUTION.md
      - create .supram-oss/BRD.md
      - create .supram-oss/ARCHITECTURE.md
      - create .supram-oss/technology.yaml (start from templates/technology.example.yaml)
      - create .supram-oss/design-registry.yaml
      - create .supram-oss/SLICE_LOG.md
      - create .supram-oss/PHASES.md
      - create .supram-oss/phases/active/ and .supram-oss/phases/archive/
      - create .supram-oss/specs/active/ and .supram-oss/specs/done/
  - record_generation: the Supram engine records product, lineage, schema, and release in the project state directory (`.supram-oss/manifest.json`; `.supram/manifest.json` after migrating to the paid product)
  - if_brownfield: convert existing agents.md, claude.md, .cursorrules, or other agent files to .supram-oss/CONSTITUTION.md
  - symlink_agent_configs: symlink claude.md, .cursorrules, .clinerules to ./AGENTS.md
  - derive_constitution: from project analysis + user input (default release_policy.strategy to local_merge, but allow user to opt into pull_request or direct)
  - prompt_testing_level: ask user for project-wide testing level (S, M, or L, default S) and persist in CONSTITUTION.md
  - derive_brd: from PRD/docs/conversation (Use templates/big-picture/BRD.md to ensure rich markdown structure)
  - derive_architecture: from code structure/docs/conversation (Use templates/big-picture/ARCHITECTURE.md and include mermaid diagrams)
  - derive_design_registry: create an empty .supram-oss/design-registry.yaml for project-specific additions
  - derive_technology: from detected stack, using templates/technology.example.yaml as the starting shape
  - initialize_phase_index: create derived .supram-oss/PHASES.md
  - initialize_slice_log: create .supram-oss/SLICE_LOG.md
  - derive_runtime_smoke: add the cheapest real-entry-point smoke check to the project integration suite for executable applications
  - present: all docs to human for review
  - wait: human approval
  - commit: initial supram-oss setup
  - report_next_command: Derive the earliest permitted command from the project filesystem state
  - stop_after_init: Return control to the user without invoking another workflow command

must_do:
  - Get human approval on all docs
  - Copy exactly the folders and state files enumerated in `create_scaffold`
  - Preserve all existing agent rules (claude.md, .cursorrules, etc.) into CONSTITUTION.md
  - Symlink all alternative agent config files to AGENTS.md
  - Explain what was derived and why
  - Record the supram-oss-skills source revision and installed skill digests
  - Use clarification answers only for bootstrap artifacts and configuration
  - Stop after reporting the next permitted command

must_not_do:
  - Skip human review
  - Guess without asking
  - Overwrite existing files without asking
  - Create application implementation files
  - Interpret a requested clarification answer as authorization for another command
  - Invoke define, design, tasks, build, verify, or release automatically
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Supram engine (paid offering). -->
