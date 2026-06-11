---
status: promoted
lane_est: normal
source: plan-0001 / 2026-06-11 session on plan-artifact consolidation
created: 2026-06-11
promoted_to: plan-0002 (shipped as the rv:brainstorm skill)
---

# Backlog: `rv:plan` skill — crystallize a plan from a brainstorm session

> Work you have **decided to defer**, not drop — not now, but worth keeping so a later session
> doesn't have to rethink it.

## What

A core skill (`rv:plan`, shipped on install like `rv:recap`) that turns the current brainstorm
conversation into a `draft` plan in `docs/plans/`: summary, main flow, technical design, local
decisions, implementation steps — ready for the human to review and flip to `approved`. It is the
"log plan" step of the loop (*brainstorm → log plan → review plan → implement → recap*), the
front-end counterpart of `rv:recap` (which crystallizes at the end).

## Why deferred

The 0.3.0 release already carries a breaking rename; adding a new skill in the same release mixes
two concerns. Also worth watching how the plan template feels in real use first.

## Pick-up trigger

After the plan format has been used for a few real changes (≥2–3 plans written by hand) and the
sections have stabilized — or when manually copying the template starts to feel like friction.

> **Picked up 2026-06-11** (same day — the human asked for it directly): shipped as
> **`rv:brainstorm`**, scoped wider than sketched here — it covers the whole lifecycle entry
> (context recall → intake → options brainstorm), not just plan generation. See
> [plan-0002](../plans/0002-rv-brainstorm-skill.md).

## Notes

- Spec Kit's `/specify` + `/plan` commands are the closest prior art; ours should stay
  one-command and Markdown-only.
- Should number the file, fill `Related ADRs` from any ADR the session produced, and read out the
  intake Output block it implies.
