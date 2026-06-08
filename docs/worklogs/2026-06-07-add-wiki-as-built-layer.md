---
date: 2026-06-07
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [ADR-0002]
files_changed:
  - docs/decisions/0002-llm-wiki-as-built-layer.md
  - templates/wiki.md
  - framework/02-lifecycle.md
  - framework/03-artifacts.md
  - framework/04-conventions.md
  - framework/06-intake.md
  - AGENTS.md
  - README.md
  - docs/wiki/.gitkeep
friction: none
---

# Worklog: 2026-06-07 — Add `wiki` (the per-topic as-built layer)

## Done

- Brainstormed out a gap: no artifact answers "how does capability X *currently* work". A spec is
  grained by *change*, a worklog by *session*, an ADR is a *decision* — none is the as-built reference
  of a *topic*. The Ship phase locks in no capability document.
- Locked in a 9th artifact type `wiki` (per-topic, overwritten in place) instead of making the spec live.
  Recorded **ADR-0002**.
- Added `templates/wiki.md` (fresh frontmatter: `updated`/`verified_against_code`/`source_specs`).
- Patched the framework: `03-artifacts` (table + definition + the spec↔wiki boundary), `02-lifecycle`
  (phase 6 Ship: the wiki-update step is **mandatory**), `06-intake` (read the wiki first + a `Wiki` line
  in the Output block), `04-conventions` (storage + naming by topic slug, not numbered).
- Updated `AGENTS.md` (artifact table + protocol steps 2/5) and `README.md` (the structure tree).

## In-place decisions

- The spec↔wiki boundary is stated by **two axes** (scope + answer), not by metaphor — because
  "minutes/receipt/diff-HEAD" are all loaned words you have to un-load. → locked in ADR-0002.
- The spec does **not** become a living document (rejected Option A): avoids forcing the spec into two roles and the wrong grain.
- The wiki is named `<topic-slug>.md`, not `NNNN` — looked up by topic, not by timeline.
- Doc-rot is treated by two intrinsic mechanisms: a forced update at the Ship gate + fresh frontmatter.
- The `riverflow-capture` skill stays as is (only handles ADR + worklog) — wiki belongs to the Ship phase, outside capture's scope.

## Still open

- [ ] No real wiki page yet (`docs/wiki/` only has `.gitkeep`) — it will be born from the first real capability ship.
- [ ] `verified_against_code` relies on human/agent discipline; there is no automated way to detect a stale page yet (intended: riverflow adds no tooling).

## Next step

Next session that ships a real capability: create the first `docs/wiki/<topic>.md` to validate the template + the Ship gate work in practice.
