---
date: 2026-06-11
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [plan-0001, ADR-0004, backlog-0001]
files_changed:
  - docs/plans/ (renamed from docs/specs/)
  - docs/templates/plan.md (new, replaces spec.md; task.md deleted)
  - docs/framework/{02-lifecycle,03-artifacts,04-conventions,05-risk,06-intake}.md
  - docs/framework/{VERSION,CHANGELOG.md} (0.3.0)
  - docs/templates/{user-story,decision,worklog,backlog,wiki}.md
  - AGENTS.md
  - README.md
  - .claude/skills/rv:recap/SKILL.md
  - .claude/skills/rv:update-version/SKILL.md
  - docs/decisions/0004-plan-artifact-consolidation.md
  - docs/backlogs/0001-rv-plan-skill.md
friction: none
---

# Worklog: 2026-06-11 — Consolidate the artifact catalog around `plan`

> Released as **0.3.0** (breaking; pre-1.0 breaking lands in MINOR — see CHANGELOG migration note).

## Done

- Discussed (VN conversation) where a technical design plan lives in riverflow → concluded the
  catalog should consolidate: `spec` + `task` → `plan`; compared against Spec Kit / Kiro / BMAD.
- Renamed `docs/specs/` → `docs/plans/`; new `plan.md` template with Technical design, Decisions
  (+ promotion rule to ADR), Implementation steps (checkbox), status `draft → approved → done`.
- Removed the `task` type; raised the ADR bar to "outlives one change"; stories now explicitly
  optional by lane.
- Updated framework docs 02–06, AGENTS.md, README, remaining templates, `rv:recap` (alias
  `rv:recap spec` kept) and `rv:update-version` (migration note).
- Dogfooded: ADR-0004 (accepted), plan-0001 (`done`, first plan in the new format),
  backlog-0001 (`rv:plan` skill idea).
- Bumped VERSION to 0.3.0 + CHANGELOG entry with migration steps.

## In-place decisions

Recorded in [plan-0001 → Decisions](../plans/0001-plan-artifact-consolidation.md): brief lives in
`docs/plans/`; deprecated alias kept; breaking-in-MINOR pre-1.0. The cross-change decision is
[ADR-0004](../decisions/0004-plan-artifact-consolidation.md).

## Still open

- [ ] `rv:plan` skill → [backlog-0001](../backlogs/0001-rv-plan-skill.md)

## Next step

Commit + tag `v0.3.0` (human confirms push). Next real change should exercise the new plan
template end-to-end (draft → approved → done) to validate the format.
