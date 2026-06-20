---
status: open
lane_est: normal
source: plan-0003 / 2026-06-20 brainstorm on the rv:playbook skill
created: 2026-06-20
---

# Backlog: graduate `rv:playbook` into riverflow framework core

> Work you have **decided to defer**, not drop — kept so a later session doesn't re-think it.

## What

Promote the standalone `rv:playbook` skill (shipped first by [plan-0003](../plans/0003-rv-playbook-skill.md))
into framework core, the way `rv:brainstorm` graduated:

- Add **playbook** as an artifact type in `docs/framework/03-artifacts.md` (and the "which artifact"
  table in the framework overview / project AGENTS.md), with `docs/playbooks/` as its home.
- Write an **ADR** recording *playbook as a portable, cross-project artifact category* (the one
  decision kept local in plan-0003 that outlives that change).
- Wire distribution: README skills tree + install prompt, `rv:update-version` copy list,
  `rv:release` boundaries list.
- Ship `docs/framework/templates/playbook.md` as a first-class template.

## Why deferred

The doc form and the distillation quality should prove out on a few real conversations first
(per the brainstorm choice: standalone-first to de-risk a framework change). Changing the framework
catalog is a MINOR version bump and shouldn't ride on an unproven artifact.

## Pick-up trigger

After `rv:playbook` has produced ≥2–3 playbooks that were actually reused (copied into another
project and followed), and the template sections have stabilized.

## Notes

- Consider at graduation: the doc→skill graduation path (a proven playbook → executable `SKILL.md`)
  — currently out of scope, but the natural next rung if playbooks get re-run often.
- Keep `rv:playbook` distinct from `rv:recap` in the catalog wording: recap = project intent
  (what/why), playbook = portable process (how).
