# Changelog — riverflow

All notable changes to the riverflow framework. The current version lives in
[`VERSION`](VERSION) (single source of truth). The `rv:update-version` skill reads this file to
show *what changed* between the installed version and the latest on GitHub.

Format: [Keep a Changelog](https://keepachangelog.com/), versions follow [SemVer](https://semver.org/).

- **MAJOR** — a breaking change to a convention (renamed/removed artifact type, changed file
  layout, changed frontmatter contract) that an installed repo must migrate to.
- **MINOR** — a new artifact type, skill, template, or framework doc, backward compatible.
- **PATCH** — wording fixes, clarifications, typos — no behavior change for an installed repo.

## [0.2.0] — 2026-06-09

### Added
- **`rv:recap` now crystallizes the intent layer** — a bare `rv:recap` also writes a thin `spec` +
  `story` (with a Proof table seeded from the session's real test/QA evidence), alongside ADR +
  worklog + wiki. This is the fix for **emergent** work: you don't write the spec first, recap
  crystallizes it at the end from what was actually built.
- **New targeted parts:** `rv:recap spec` and `rv:recap story`.
- **Emergence gate:** spec/story is auto-created only when the session built a real feature (the
  "can I tell it's done without reading the code?" test); a one-off tiny patch gets none. Existing
  specs/stories are **updated**, not duplicated.

### Changed
- A bare `rv:recap` is now the full package **spec/story (if emergent) + ADR + worklog + wiki**
  (previously ADR + worklog + wiki). Backward compatible — no trigger renamed, no artifact removed.

## [0.1.1] — 2026-06-09

### Changed
- **Skill triggers are now prefixed.** `riverflow-capture` → **`rv:recap`**, `riverflow-update` →
  **`rv:update-version`** (slash commands `/rv:recap`, `/rv:update-version`). An installed repo
  picking up this version gets the renamed skill folders; the old folders can be removed.

### Added
- **`rv:recap` now recaps the wiki too** (third artifact type, alongside ADR + worklog) — closes
  the phase-6 "update the wiki on ship" gap.
- **Targeted recap from the prefix:** `rv:recap wiki` / `rv:recap worklog` / `rv:recap adr` do just
  that slice; a bare `rv:recap` does the full package.

## [0.1.0] — 2026-06-09

First versioned release. Establishes the baseline an installed repo can compare against.

### Added
- **Self-versioning.** `docs/framework/VERSION` is the single source of truth, namespaced under
  the framework dir so it travels with `docs/framework/` on install and never collides with a host
  project's own root `VERSION`.
- **`rv:update-version` skill** — clones the GitHub repo into a temp dir, semver-compares the
  remote `VERSION` against the installed one, shows the changelog delta, and suggests the update.
- This `CHANGELOG.md`.

### Baseline (already present before versioning)
- Framework docs `00`–`06` (overview, roles, lifecycle, artifacts, conventions, risk, intake).
- Templates for every artifact type.
- `rv:recap` skill (formerly `riverflow-capture`).
- `backlog` per-item artifact ([ADR-0001](../decisions/0001-backlog-per-item-files.md)).
- LLM `wiki` as-built layer ([ADR-0002](../decisions/0002-llm-wiki-as-built-layer.md)).
