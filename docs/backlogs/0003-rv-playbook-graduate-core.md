---
status: open
lane_est: normal
source: plan-0005 / 2026-06-20 brainstorm on the rv:playbook skill
created: 2026-06-20
---

# Backlog: graduate `rv:playbook` into riverflow framework core

> Work you have **decided to defer**, not drop — kept so a later session doesn't re-think it.

> **Update (2026-06-20):** distribution is **done** — the human reversed the standalone-first call;
> `rv:playbook` now ships on install (`install: true`) via the marker mechanism in
> [ADR-0006](../decisions/0006-skill-install-marker.md). What remains is the **framework-catalog**
> graduation only (the two ticked items below are complete).

## What

Promote `rv:playbook` (shipped by [plan-0005](../plans/0005-rv-playbook-skill.md)) into the framework
catalog, the way `rv:brainstorm` graduated:

- [ ] Add **playbook** as an artifact type in `docs/framework/03-artifacts.md` (and the "which
  artifact" table in the framework overview / project AGENTS.md), with `docs/playbooks/` as its home.
- [ ] Write an **ADR** recording *playbook as a portable, cross-project artifact category*.
- [x] Wire distribution — done via [ADR-0006](../decisions/0006-skill-install-marker.md): the
  `install:` marker drives README prompts + `rv:update-version` + `rv:release`, so no per-skill list.
- [x] Ship `docs/framework/templates/playbook.md` as a first-class template (already in place).

## Why deferred

The remaining catalog graduation (making `playbook` an official artifact type) is a MINOR version
bump and benefits from the doc form proving out on a few real conversations first. The skill
shipping does not require it — it just produces a `docs/playbooks/` file.

## Pick-up trigger

After `rv:playbook` has produced ≥2–3 playbooks that were actually reused (copied into another
project and followed), and the template sections have stabilized.

## Notes

- Consider at graduation: the doc→skill graduation path (a proven playbook → executable `SKILL.md`)
  — currently out of scope, but the natural next rung if playbooks get re-run often.
- Keep `rv:playbook` distinct from `rv:recap` in the catalog wording: recap = project intent
  (what/why), playbook = portable process (how).
