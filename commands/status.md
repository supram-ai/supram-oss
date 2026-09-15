---
name: harness-status
description: Display current project state

persona: Manager
reason: Read-only operation
runtime: Supram engine (status, JSON, and static HTML output)

gates:
  - check: .supram-oss/ exists
    on_fail: STOP, run /h:init

actions:
  - read: version.txt
  - read: .supram-oss/PHASES.md
  - read: .supram-oss/SLICE_LOG.md
  - scan: specs/active/*
  - scan: deferred.md in active features
  - read: .supram-oss/handover.yaml (if exists; written by the Supram engine; `.supram/` after migration)
  - display_formatted_status_report: version, phases progress, active specs, deferred item counts, recent commits, handover view
  - render_json: output raw status JSON to stdout if --json is passed (Supram engine)
  - render_html: write static HTML status to .supram-oss/status/index.html if --html is passed (Supram engine)

must_do:
  - Show complete state snapshot
  - Include last 3 SLICE_LOG entries
  - Support JSON output via --json
  - Support self-contained static HTML output via --html
  - Render current derived handover state (Supram runtime)

must_not_do:
  - Modify anything
  - Run tests or a build
  - Make network calls
  - Start a server
  - Invoke LLM review
  - Open a subprocess that might download dependencies
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Supram engine (paid offering). -->
