# Plan-0002: `rv:brainstorm` skill — the lifecycle's front door

- **Status:** done
- **Related brief:** —
- **Related stories:** — (framework change; the "user" is the human running the lifecycle)
- **Related ADRs:** [ADR-0004](../decisions/0004-plan-artifact-consolidation.md) (defines the plan artifact this skill produces)

> Picked up from [backlog-0001](../backlogs/0001-rv-plan-skill.md) at the human's direct request —
> trigger overridden deliberately. Approval of intent came from that request; written and shipped
> in the same session.

## Summary

A core skill (`rv:brainstorm <topic>`, shipped on install) that walks a problem **into** the
riverflow lifecycle before any code exists: recall what the repo already knows (wiki/ADRs/plans/
backlogs), run intake, facilitate an options-and-trade-offs brainstorm with the human, and
crystallize a **`draft` plan** in `docs/plans/` for the human to flip to `approved`. It is the
front half of the loop — *brainstorm → log plan* — with `rv:recap` as the back half. Compared to
the backlogged `rv:plan` sketch, the scope is wider: lifecycle entry (discovery + intake +
brainstorm), not just plan generation — hence the name.

## Main flow

1. `rv:brainstorm <topic>` → detect repo layout (standard / embedded / pre-0.3.0).
2. Context recall: wiki page, constraining ADRs, prior plans/stories, matching backlog items;
   summarized back in 3–5 lines, conflicts surfaced.
3. Intake: input type + lane + Output block. `tiny` → exit, no plan; hard gate → flag high-risk.
4. Facilitated brainstorm: problem → 2–3 real options → pressure-test → converge; human owns intent.
5. Crystallize: `draft` plan (+ `proposed` ADRs, story if warranted, backlog items for off-scope
   ideas; graduate any picked-up backlog item).
6. Hand off: state the gate — human reviews and says the word to mark `approved`; no implementing
   in the same breath.

## Edge cases & error states

| Situation | Expected behavior |
| --- | --- |
| Topic already has a plan/backlog item | update/graduate it, never shadow with a duplicate |
| Work classifies as `tiny` | say "plan not needed", offer the direct patch |
| Existing ADR contradicts the idea | surface it in recall: brainstorm within it or propose superseding |
| Large new product goal, not one change | draft a `product-brief` before/instead of a plan |
| No riverflow layout in the repo | ask whether to initialize before continuing |

## Constraints

- Pure Markdown + conversation; no tooling (riverflow's maturity stance).
- The skill must end at `draft` — flipping to `approved` is the human's move (`01-roles.md`).
- Proof table in a brainstormed plan records required tiers only, never invented evidence.

## Technical design

One new file `.claude/skills/rv:brainstorm/SKILL.md` (procedure: layout detect → recall → intake
→ facilitate → crystallize → hand off, plus boundaries). Distribution wiring: README (skills
tree, install prompt, How-to-use loop), `rv:update-version` copy list, `rv:release` boundaries
list (still core-only itself). No framework-doc changes needed — the skill executes phases 1–2 of
`02-lifecycle.md` as written.

## Decisions

- Named `rv:brainstorm` (not `rv:plan` as backlogged) — the skill's value is the structured entry
  into the lifecycle, of which the plan file is the output, not the activity itself.
- Shipped on install (third consumer skill) rather than core-only — the loop's front half belongs
  to every project applying riverflow.
- Backlog-0001's pick-up trigger ("after 2–3 hand-written plans") consciously overridden by the
  human's direct request; noted on the backlog item rather than silently ignored.

## Implementation steps

- [x] Write `.claude/skills/rv:brainstorm/SKILL.md`
- [x] Add to `rv:update-version` copy list + `rv:release` boundaries
- [x] README: skills tree, install prompt, How-to-use loop (brainstorm ↔ recap as the two halves)
- [x] Graduate backlog-0001 (`promoted` → plan-0002)
- [x] This plan + worklog
- [x] Bump VERSION → 0.4.0 + CHANGELOG entry

## Acceptance criteria

- [x] `/rv:brainstorm` exists as a shipped-on-install skill and ends at a `draft` plan with an
      explicit human-approval handoff
- [x] All three distribution surfaces (README install prompt, `rv:update-version`, `rv:release`
      boundaries) list the same shipped-skill set
- [x] Backlog-0001 is `promoted` with a trace, not deleted

## Proof

| Tier | Needed? | Evidence |
| --- | --- | --- |
| Unit | no | docs-only change |
| Integration | yes | `grep -rn "rv:brainstorm" README.md .claude/skills` → consistent across all surfaces; skill registered in the session's skill list |
| E2E | not yet | first real `rv:brainstorm <topic>` run on an actual change will validate the flow |

## Out of scope

- Changing framework docs `00`–`06` (the skill follows the lifecycle as already written).
- A matching `rv:implement` skill (executing an approved plan) — not requested; revisit if the
  loop's middle leg also wants a trigger.
