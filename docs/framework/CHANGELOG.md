# Changelog — riverflow

All notable changes to the riverflow framework. The current version lives in
[`VERSION`](VERSION) (single source of truth). The `rv:update-version` skill reads this file to
show *what changed* between the installed version and the latest on GitHub.

Format: [Keep a Changelog](https://keepachangelog.com/), versions follow [SemVer](https://semver.org/).

- **MAJOR** — a breaking change to a convention (renamed/removed artifact type, changed file
  layout, changed frontmatter contract) that an installed repo must migrate to.
- **MINOR** — a new artifact type, skill, template, or framework doc, backward compatible.
- **PATCH** — wording fixes, clarifications, typos — no behavior change for an installed repo.

## [0.7.0] — 2026-06-19

### Added
- **`roadmap` — the per-product forward-looking layer** (new artifact type, the catalog's 9th).
  It closes the one empty cell in the catalog: *product scope, living, future direction*. A page
  `docs/roadmap/<product>.md` holds a stable north-star, `Now / Next / Later` themes with status,
  and a gap-to-vision read — the product-level mirror of the wiki (wiki = *where we are*, roadmap =
  *where we're going*). It is the **top-down source that refills the backlog**: when the backlog
  runs dry or the vision changes, diff the roadmap against the wiki and graduate new backlog items.
  **Optional by scale** (a single-feature product stays on its `product-brief`). Slug-named, in its
  own `docs/roadmap/` dir, overwritten in place, refreshed at the Ship gate. → [ADR-0005](../decisions/0005-product-roadmap-forward-layer.md), [plan-0004](../plans/0004-product-roadmap-forward-layer.md)
- **New template** `docs/framework/templates/roadmap.md`, and a self-hosted example at
  `docs/roadmap/riverflow.md` (the framework's own roadmap).
- **Plan `Serves:` back-link** — a plan that advances a roadmap theme names it; the roadmap holds
  theme status as the single source of truth (no two-place sync).
- An installed repo picking this up gets the new template via the `rv:update-version` copy step;
  create `docs/roadmap/<product>.md` from the template when the product spans multiple themes.

### Changed
- **Lifecycle gains a top-down feed.** `02-lifecycle` documents the roadmap sitting *above* the
  loop (`roadmap → backlog → story/plan → ship → refresh roadmap`), referenced in phase 1
  (Discovery refills the backlog from it) and phase 6 (Ship refreshes theme status).
- **Intake Output block gains a `Roadmap:` line** (which theme this work serves, refreshed on ship;
  "n/a" for single-feature/infra work), with a note to refill a dry backlog from the roadmap rather
  than invent work.

## [0.6.0] — 2026-06-19

> **Breaking** (pre-1.0, breaking lands in MINOR): templates moved from the top-level
> `docs/templates/` into `docs/framework/templates/`. Migration for an installed repo:
> `git mv docs/templates docs/framework/templates` (or let `/rv:update-version` do it — its copy
> step now brings templates in inside `docs/framework/` and removes the orphaned `docs/templates/`).
> No artifact instances change; only the location of the blank templates moves.

### Changed
- **Templates nested under the framework** (`docs/templates/` → `docs/framework/templates/`). The
  whole riverflow "core" (rules + the blank forms those rules define) now lives under one root, so
  `docs/` carries a clean invariant: **`docs/framework/` = riverflow itself; everything else =
  your artifacts.** Chosen as a nested subfolder (not flat `framework/template_*.md`) to keep the
  prose-rules vs blank-form boundary. → [plan-0003](../plans/0003-nest-templates-under-framework.md)
- **Install + `rv:update-version` copy collapses from two steps to one** — templates ride inside
  `docs/framework/`, so copying the framework brings them along. `rv:update-version` gained a
  one-time relocation step that removes an existing install's orphaned `docs/templates/`.

## [0.5.1] — 2026-06-14

### Changed
- **Language convention now defaults to English.** The `AGENTS.md` "Language" section is simplified:
  chat replies are English by default (set `COMMUNICATE=vi` in `.env` for Vietnamese), and artifacts
  (code, comments, Markdown docs, `SKILL.md`) are always English. Mirror the user if they write in
  another language. No structural change — installs picking this up just get the clearer wording and
  the English default.

## [0.5.0] — 2026-06-13

### Added
- **Language convention in `AGENTS.md`.** A new "Language" section splits chat language (follow
  `COMMUNICATE` in `.env` — `vi`/`en`, mirror the user) from artifact language (code, comments,
  committed Markdown, `SKILL.md` default to English). Installs picking up this version inherit the
  rule; add a `.env` with `COMMUNICATE=` (gitignored) to set the chat language per project.
- **coflow → riverflow migration guide** at [`docs/wiki/coflow_migration.md`](../wiki/coflow_migration.md)
  — a paste-ready prompt that upgrades a coflow repo in place (framework/templates/skills + versioning)
  while leaving every `docs/` artifact untouched. The README links to it instead of inlining the prompt.
## [0.4.0] — 2026-06-11

### Added
- **`rv:brainstorm` skill** (shipped on install, third consumer skill) — the lifecycle's front
  door: `rv:brainstorm <topic>` recalls what the repo already knows (wiki/ADRs/plans/backlogs),
  runs intake, facilitates an options brainstorm with the human, and crystallizes a **`draft`
  plan** for the human to flip to `approved`. Front half of the loop (*brainstorm → log plan*);
  `rv:recap` remains the back half. → [plan-0002](../plans/0002-rv-brainstorm-skill.md)
- An installed repo picking this up gets the new skill via the `rv:update-version` copy step
  (the copy list now includes `.claude/skills/rv:brainstorm/`).

## [0.3.0] — 2026-06-11

> **Breaking** (pre-1.0, breaking lands in MINOR): the artifact catalog is consolidated around
> `plan`. Migration for an installed repo: `git mv docs/specs docs/plans`, replace
> `docs/templates/spec.md` + `docs/templates/task.md` with the new `docs/templates/plan.md`,
> and use `source_plans` in new wiki pages. Existing frozen artifacts need no rewrite.

### Changed
- **`spec` → `plan`** (`docs/specs/` → `docs/plans/`, template `spec.md` → `plan.md`). The name
  now matches the artifact's lifecycle (intent of one change, frozen after ship) and the real
  loop: *brainstorm → log plan → review plan → implement → recap*. `product-brief` lives in
  `docs/plans/` too. → [ADR-0004](../decisions/0004-plan-artifact-consolidation.md)
- **The plan absorbs the technical design and the work breakdown** — new sections: `Technical
  design`, `Decisions` (choices local to the change, with a promotion rule to ADR), and
  `Implementation steps` (a checkbox list a later session can resume mid-plan).
- **Plan status is the review gate:** `draft → approved → done`. Lane `normal`+ requires a
  human-approved plan before implementation (lifecycle phase 2 is now *Plan*).
- **ADR bar raised:** an ADR is only for a choice that **outlives one change** ("would anyone
  need to look this up six months from now, on other work?"); change-local choices live inline
  in the plan's Decisions section.
- **Stories are explicitly optional by lane** (`tiny` needs none); the story remains the
  demand-side tracking unit, the plan the supply side.
- `rv:recap` recaps `plan`/`story` (trigger `rv:recap plan`; `rv:recap spec` kept as a
  deprecated alias). Wiki frontmatter `source_specs` → `source_plans`.

### Removed
- **The `task` artifact type** (`docs/templates/task.md`). The plan's Implementation steps
  checklist is the work breakdown; externally tracked work links its Jira key from the plan.

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
