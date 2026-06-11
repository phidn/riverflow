# Plan-0001: Consolidate the artifact catalog around `plan`

- **Status:** done
- **Related brief:** —
- **Related stories:** — (framework change, no user-facing story)
- **Related ADRs:** [ADR-0004](../decisions/0004-plan-artifact-consolidation.md)

> First plan written in the new format — dogfooding the change it describes.

## Summary

Rename `spec` → `plan` and fold the `task` type into the plan, so one document per change holds
the what (flows, acceptance, proof) and the how (technical design, local decisions,
implementation steps). The framework's nouns now match the real loop: *brainstorm → log plan →
review plan → implement → recap*. Direction rationale lives in ADR-0004.

## Main flow

1. A change worth more than a tiny patch gets a `plan` in `docs/plans/` (`draft`).
2. A human reviews it; status flips to `approved` (required for lane `normal`+) before coding.
3. Implementation ticks the plan's checklist; proof lands in the Proof table.
4. On ship the plan flips to `done` and freezes; current state settles in the wiki.

## Edge cases & error states

| Situation | Expected behavior |
| --- | --- |
| Installed repo (pre-0.3.0) updates | `rv:update-version` points out the migration: `git mv docs/specs docs/plans`, drop stale `spec.md`/`task.md` templates |
| Old trigger `rv:recap spec` | still works as a deprecated alias for `rv:recap plan` |
| Decision misjudged as local | promote it from the plan's Decisions section to its own ADR when a later plan needs it |

## Constraints

- Pure Markdown, no tooling — riverflow's maturity stance holds.
- Frozen artifacts (ADR-0001…0003, old worklogs) keep their original "spec" wording — history is
  not rewritten.

## Technical design

Docs-only change across the framework:

- `docs/specs/` → `docs/plans/` (brief lives there too); template `spec.md` → `plan.md` with new
  sections **Technical design**, **Decisions** (+ promotion rule), **Implementation steps**
  (checkbox), and status `draft | approved | done` as the review gate. `task.md` deleted.
- Framework docs `02`–`06` + `AGENTS.md` + `README.md` reworded: phase 2 is now *Plan*, lane
  tables require an approved plan for `normal`+, ADR bar raised to "outlives the change".
- Wiki frontmatter `source_specs` → `source_plans`.
- Skills: `rv:recap` recaps plan/story (alias kept); `rv:update-version` carries the migration
  note.

## Decisions

- Chose `docs/plans/` for `product-brief` over a separate directory — a brief is intent too,
  frozen once approved; one directory fewer.
- Kept `rv:recap spec` as a deprecated alias rather than dropping the trigger — installs picking
  up 0.3.0 shouldn't break muscle memory in one release.
- Released as MINOR (0.3.0) although the rename is breaking — pre-1.0, breaking lands in MINOR;
  the CHANGELOG entry flags it as breaking with migration steps.

## Implementation steps

- [x] `git mv docs/specs docs/plans`; `spec.md` → `plan.md` (new sections), delete `task.md`
- [x] Rewrite `03-artifacts.md` (catalog, Plan ↔ Wiki boundary, ADR bar, story-optional)
- [x] Update `02-lifecycle.md` (phase 2 = Plan, approval gate, no task)
- [x] Update `04-conventions.md` (storage, plan status, examples, handoff)
- [x] Update `05-risk.md` + `06-intake.md` (lane tables, thin plan, Output block)
- [x] Update `AGENTS.md` + `README.md`
- [x] Update templates: user-story, decision, worklog, backlog, wiki (`source_plans`)
- [x] Update skills: `rv:recap` (plan vocabulary + alias), `rv:update-version` (migration note)
- [x] Write ADR-0004, this plan, backlog-0001 (`rv:plan` skill), worklog
- [x] Bump VERSION → 0.3.0 + CHANGELOG entry with migration note

## Acceptance criteria

- [x] No live framework file (docs/framework, templates, skills, AGENTS.md, README) still refers
      to `docs/specs/`, the `spec` artifact type, or the `task` type — frozen artifacts excepted
- [x] The plan template encodes the review gate (`approved`) and a resumable checklist
- [x] An installed repo crossing 0.3.0 gets the migration spelled out (CHANGELOG +
      `rv:update-version`)

## Proof

| Tier | Needed? | Evidence |
| --- | --- | --- |
| Unit | no | docs-only change |
| Integration | yes | `grep -rn "docs/specs\|spec-0\|templates/spec\|templates/task" docs/framework docs/templates .claude/skills AGENTS.md README.md` → no live references |
| E2E | no | — |

## Out of scope

- `rv:plan` skill (crystallize a plan from a brainstorm session) → [backlog-0001](../backlogs/0001-rv-plan-skill.md)
- Rewriting frozen artifacts (ADR-0001…0003, shipped worklogs) to the new vocabulary
