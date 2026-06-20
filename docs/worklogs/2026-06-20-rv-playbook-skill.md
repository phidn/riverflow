---
date: 2026-06-20
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [plan-0005, backlog-0003, playbook-0001]
files_changed:
  - .claude/skills/rv:playbook/SKILL.md
  - docs/framework/templates/playbook.md
  - docs/plans/0005-rv-playbook-skill.md
  - docs/backlogs/0003-rv-playbook-graduate-core.md
  - docs/playbooks/0001-author-a-riverflow-skill.md
friction: none
---

# Worklog: 2026-06-20 — rv:playbook skill

> Built a standalone skill that distills a conversation's reusable process into a portable playbook.

## Done

- Ran `rv:brainstorm` on the idea "turn a long conversation into a reusable workflow doc". Recall +
  intake (lane `normal`, no hard gate) → surfaced the core tension (where it lives + what form) →
  asked 3 design-forking questions. Human chose: **descriptive doc only**, **in-project (copy to
  reuse)**, **standalone-first then graduate to core**, and preferred the name **playbook**.
- Crystallized [plan-0005](../plans/0005-rv-playbook-skill.md) (`draft`→`approved`→`done`) and
  [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md) (graduation deferred).
- Implemented `docs/framework/templates/playbook.md` and `.claude/skills/rv:playbook/SKILL.md` (procedure:
  detect → identify process → supplement from docs → distill/generalize → write → hand off; plus a
  Boundaries section and an explicit "why this is NOT rv:recap" table).
- Smoke-tested by running the skill against this very session → produced
  [playbook-0001](../playbooks/0001-author-a-riverflow-skill.md) ("Author a new riverflow skill").
- Proof: grep confirms no framework distribution surface was touched (standalone-first); skill is
  registered in the session skill list.

## In-place decisions

- Named it `playbook` not `workflow` — "workflow" collides with Claude Code's Workflow
  orchestration; "playbook" = read-and-follow. (In plan-0005 Decisions.)
- Kept "playbook as a portable artifact category" as a **local** decision, to be promoted to an ADR
  at graduation rather than now. (In plan-0005 Decisions + backlog-0003.)

## Still open

- [ ] Graduate `rv:playbook` into framework core (ADR + `03-artifacts.md` catalog + README /
      `rv:update-version` / `rv:release` wiring) — [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md),
      pick up after ≥2–3 playbooks are reused.
- [ ] Possible later rung: doc→skill graduation (a proven playbook → executable `SKILL.md`).

## Next step

Use `rv:playbook` on a real domain conversation (e.g. a BRD→design session) and copy the result
into another project to validate the "portable, re-runnable" claim before graduating it.
