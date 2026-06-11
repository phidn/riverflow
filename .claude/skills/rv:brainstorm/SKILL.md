---
name: rv:brainstorm
description: Enter the riverflow lifecycle at the front door — before any plan or code exists. Facilitates a structured brainstorm of a problem/idea (context recall from wiki/ADRs/plans, intake classification, options + trade-offs, pressure-testing) and crystallizes the result into a draft plan in docs/plans/ (plus proposed ADRs / backlog items when they surface), ready for the human to review and flip to approved. This is the "brainstorm → log plan" front half of the loop; rv:recap is the back half. Canonical trigger is the prefix "rv:brainstorm" followed by the topic/problem (e.g. "rv:brainstorm offline mode for the editor", "/rv:brainstorm"). Also fires on natural phrasing like "let's brainstorm X into a riverflow plan". Use when the user wants to think a change through with the framework before implementing — NOT when they ask for a direct fix or already have an approved plan.
---

# rv:brainstorm

Walks a problem **into** the riverflow lifecycle: recall what already exists, classify the work
(intake), brainstorm options with the human, and **crystallize a `draft` plan** for them to
review. It is the front half of the loop — *brainstorm → log plan → review plan → implement →
recap* — and the counterpart of `rv:recap` (which crystallizes at the end of emergent work).

Source framework: the riverflow repo (`docs/framework/`, `docs/templates/`).
Key principle here: **reviewing a plan is cheaper than reviewing a diff** — this skill exists to
put a reviewable plan in front of the human before any code is written.

## When to use

- The user types **`rv:brainstorm <topic/problem>`** (or `/rv:brainstorm`), or says they want to
  brainstorm / think through / design a change with riverflow before building it.
- Do **NOT** use for: a direct tiny fix (just run intake and patch), work that already has an
  `approved` plan (go implement it), or end-of-session capture (that is `rv:recap`).

## Procedure

### 1. Detect the repo layout
Same detection as `rv:recap`: standard riverflow (`docs/plans/`, `docs/decisions/`,
`docs/stories/`, `docs/wiki/`, templates in `docs/templates/`) or embedded (root-level dirs +
`decisions.md` router). A pre-0.3.0 install may have `specs/` — treat it as the plans directory
and note the rename. No layout at all → ask whether to initialize riverflow first.

### 2. Context recall — read before thinking
Before generating any ideas, gather what the repo already knows about the topic:

- `docs/wiki/<topic>.md` — how the capability works **now** (if a page exists).
- `docs/decisions/` — ADRs that already constrain the direction. **Surface conflicts early**:
  "ADR-0002 already settled X — brainstorm within it, or supersede it?"
- `docs/plans/` + `docs/stories/` — prior or in-flight work on the same topic (update/extend an
  existing plan rather than starting a duplicate).
- `docs/backlogs/` — a deferred item on this topic? This brainstorm may be its pick-up: graduate
  it (`status: promoted`, fill `promoted_to`) when the plan is created.

Open with a 3–5 line summary of this recall so the human sees the starting state.

### 3. Intake — classify before brainstorming
Run the intake gate (`docs/framework/06-intake.md` + `05-risk.md`): input type, lane, and read
out the Output block (`Lane / Type / Docs / Wiki / Story / Proof`). Two early exits:

- **Lane `tiny`** → say a plan is not needed ("not needed — tiny patch") and offer to just do it.
- **Hard gate** (auth, data loss, audit, external vendor, removing a verification) → flag
  high-risk now, so the human knows approval will be required at every step.

The lane also calibrates the brainstorm: `normal` keeps it light (one pass over options);
`high-risk` digs into failure modes and proof depth.

### 4. Facilitate the brainstorm — human owns intent
This is a conversation, not a monologue. Work through, asking the human **focused questions, one
at a time** (don't interrogate with a wall of questions):

1. **Problem first.** What hurts, for whom, why now? What does success look like? (If this turns
   out to be a large new product goal, draft a `product-brief` from `docs/templates/product-brief.md`
   instead of — or before — a plan, and say so.)
2. **Options.** Generate 2–3 genuinely different directions with pros/cons — not one option and
   two strawmen. Anchor each against the recall from step 2.
3. **Pressure-test the front-runner.** Edge cases, constraints, blast radius, rollback story,
   what proof the lane demands.
4. **Converge.** Recommend one direction and say why in one or two sentences. The human confirms
   or redirects — their call, not yours.

Capture as you go: choices local to this change → the plan's **Decisions** section; a choice that
outlives the change → a separate ADR in `proposed`; good-but-off-scope ideas → backlog items.

### 5. Crystallize — write the draft plan
When the direction is confirmed, write the artifacts (English, or another language with correct
orthography):

- **Plan** (always, unless step 3 exited): next NNNN in the plans directory,
  `<NNNN>-<slug>.md` from `docs/templates/plan.md`. **Status: `draft`** — never higher. Fill:
  Summary, Main flow, Edge cases, Constraints, **Technical design** (deep enough to review the
  direction — affected modules, data changes, sequencing), **Decisions** (from step 4, with the
  promotion rule in mind), **Implementation steps** (unticked checklist, resumable), Acceptance
  criteria, **Proof** (mark the tiers the lane demands; evidence stays empty — nothing is built
  yet), Out of scope.
- **ADR(s)**: only for choices that outlive this change, status `proposed`, linked under the
  plan's Related ADRs.
- **Story**: if the work is user-facing and the lane warrants one, draft it (`draft`, `Related
  plan` → the new plan).
- **Backlog items**: for the off-scope ideas worth keeping; and graduate any backlog item this
  brainstorm picked up.

### 6. Hand off to review — and stop
Summarize: plan path + one-line gist, any proposed ADRs/backlog items, the lane. Then state the
gate explicitly: *"Plan is `draft` — review it and say the word to mark it `approved`; I won't
start implementing until then (lane `normal`+)."* Flipping to `approved` is the **human's** move
(`docs/framework/01-roles.md`) — on their say-so, update the status line, then implementation can
start as its own chunk of work.

## Boundaries

- **Never self-approve.** The skill ends at a `draft` plan + a clear handoff; it does not flip
  status to `approved` without the human saying so, and it does not start implementing in the
  same breath.
- **Don't skip the recall.** Brainstorming against an empty head when the repo has a wiki/ADRs on
  the topic produces conflicting plans — read first (step 2).
- **Don't duplicate.** An existing plan/backlog item on the topic gets updated/graduated, not
  shadowed by a new file.
- **Lane `tiny` → no plan.** Say so and offer the direct patch; ceremony must stay proportional.
- Keep the plan reviewable: short enough to read in one sitting; the Technical design section
  carries the weight, not prose padding.
- Proof table at this stage records **which tiers will be required**, never fabricated evidence.
