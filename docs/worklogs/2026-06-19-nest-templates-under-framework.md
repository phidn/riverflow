---
date: 2026-06-19
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [plan-0003]
files_changed:
  - docs/framework/templates/ (moved from docs/templates/ via git mv)
  - docs/plans/0003-nest-templates-under-framework.md (new)
  - docs/framework/VERSION (0.5.1 → 0.6.0)
  - docs/framework/CHANGELOG.md (0.6.0 entry)
  - README.md (tree diagram + install/migration prompts)
  - AGENTS.md
  - docs/framework/00-overview.md, 02-lifecycle.md, 03-artifacts.md
  - docs/decisions/0001-backlog-per-item-files.md, 0002-llm-wiki-as-built-layer.md
  - .claude/skills/rv:brainstorm/SKILL.md, rv:recap/SKILL.md, rv:update-version/SKILL.md
  - docs/wiki/versioning.md
friction: none
---

# Worklog: 2026-06-19 — Nest templates under framework

## Done

- Relocated `docs/templates/` → `docs/framework/templates/` with `git mv` (history preserved), so
  the whole riverflow core sits under one root. New invariant: `docs/framework/` = riverflow itself;
  everything else in `docs/` = the user's artifacts.
- Updated all **live** references to the new path: three skills, three framework docs, AGENTS.md,
  README (tree diagram + all three install/migration prompts), two ADR "Related" links. Left
  **historical** references verbatim (worklogs, prior CHANGELOG entries, frozen plans/0001).
- Reworked `rv:update-version`: the copy step now pulls templates in *inside* `docs/framework/`
  (two `cp -R` collapse to one) and gained a one-time relocation that removes an existing install's
  orphaned `docs/templates/`.
- Bumped VERSION 0.5.1 → 0.6.0 (MINOR — pre-1.0 breaking layout change, same treatment as the
  0.3.0 spec→plan rename) + CHANGELOG entry with the migration note.
- Updated `docs/wiki/versioning.md` (copy-step description + freshness frontmatter).

## In-place decisions

- Nested subfolder `docs/framework/templates/`, not flat `framework/template_*.md` — flat would
  mix fill-in forms among the numbered `00-…06-` chapters and re-clutter the folder being cleaned.
- MINOR not MAJOR — followed the framework's own pre-1.0 convention ("breaking lands in MINOR").
- Left ADR-0002's already-dead `templates/spec.md` link untouched (broken since 0.3.0, out of scope).

## Still open

- [ ] Nothing blocking. Commit/push when the human is ready (not done automatically).

## Next step

Review the diff and the 0.6.0 CHANGELOG entry, then commit. Existing installs pick up the
relocation automatically on their next `/rv:update-version`.
