---
date: 2026-06-20
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [ADR-0006, plan-0005, backlog-0003]
files_changed:
  - .claude/skills/rv:brainstorm/SKILL.md
  - .claude/skills/rv:recap/SKILL.md
  - .claude/skills/rv:playbook/SKILL.md
  - .claude/skills/rv:update-version/SKILL.md
  - .claude/skills/rv:release/SKILL.md
  - README.md
  - docs/decisions/0006-skill-install-marker.md
  - docs/plans/0005-rv-playbook-skill.md
  - docs/backlogs/0003-rv-playbook-graduate-core.md
friction: "the install set was duplicated as hand-maintained name lists in 5+ places — exactly the drift that hid rv:playbook from installs"
---

# Worklog: 2026-06-20 — install-marker mechanism + ship rv:playbook

> The README install prompt was missing `rv:playbook`; the human flagged that re-editing a list
> every time a skill is added is bad. Root cause: the install set was hardcoded in 5+ places. Fixed
> by making an `install:` frontmatter marker the single source of truth.

## Done

- **[ADR-0006](../decisions/0006-skill-install-marker.md)** — chose a boolean `install:` field in
  each SKILL.md frontmatter as the single source of truth for what ships (vs a manifest file or
  install-all-except-core).
- Added the marker to all five skills: `rv:brainstorm` / `rv:recap` / `rv:playbook` /
  `rv:update-version` → `install: true`; `rv:release` → `install: false`.
- **Reversed plan-0005's standalone-first call** (the human's decision): `rv:playbook` ships on
  install — that is its purpose. Noted on the frozen plan + backlog-0003.
- Made every distribution surface generic ("copy every skill with `install: true`"): README dir
  tree, fresh-install prompt, migrate prompt, prose paragraph, and a new How-to-use line for
  `rv:playbook`; `rv:update-version` copy block (now a frontmatter-grep loop); `rv:release`
  boundaries (now point at the marker, no name list).
- Verified: the selector ships `rv:brainstorm/recap/playbook/update-version` and skips
  `rv:release`; no hardcoded skill-name lists remain in the distribution surfaces.

## In-place decisions

- Marker semantics fail **closed**: a missing/malformed `install:` → treated as not shipped (safe).
  (In ADR-0006 Consequences.)

## Still open

- [ ] Framework-catalog graduation: add `playbook` to `docs/framework/03-artifacts.md` + the
      "which artifact" tables, and an ADR for the *playbook artifact category* —
      [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md).
- [x] Version bump — released **0.8.0** (MINOR): rv:playbook ships + the install-marker mechanism.

## Next step

Pick up the framework-catalog graduation in backlog-0003 (add `playbook` to `03-artifacts.md`)
once a few playbooks have proven out in real reuse.
