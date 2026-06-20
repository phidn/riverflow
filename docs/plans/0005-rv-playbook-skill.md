# Plan-0005: `rv:playbook` skill — distill a conversation into a reusable playbook

- **Status:** done
- **Related brief:** —
- **Related stories:** — (framework-adjacent change; the "user" is the human running the lifecycle)
- **Related ADRs:** — (the "playbook as a portable artifact category" decision is kept local for now;
  promote to an ADR when the skill graduates into framework core — see [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md))

> Status is the review gate: `draft` while brainstorming, `approved` once a human has reviewed,
> `done` when implemented with proof.

> **Update (2026-06-20, same day):** the "standalone-first, do not ship on install" decision below
> was **reversed by the human** — `rv:playbook` is meant to be installed with riverflow (that is its
> purpose). It is now marked `install: true` and ships like the other consumer skills, via the marker
> mechanism in [ADR-0006](../decisions/0006-skill-install-marker.md). The distribution items in
> [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md) are done; only the framework-catalog
> graduation (adding `playbook` to `03-artifacts.md`) remains. This plan stays frozen as the record
> of the original intent.

## Summary

A skill (`rv:playbook [name]`) that reads the **current conversation** and distills the *reusable
process* it followed — e.g. *elicit BRD → draft design → …* — into a descriptive **playbook**
Markdown file in `docs/playbooks/`. Unlike every existing riverflow artifact (all project-scoped,
recording *what/why* for one product), a playbook captures *how* a kind of work is done, written to
be **copied into another riverflow project and re-run there**. Because the session already runs on
riverflow, most steps and their per-step artifacts are recoverable straight from the conversation;
where a step is thin, the skill **supplements from existing `docs/`** (wiki/plans/ADRs). Scoped as
a **standalone skill first** (experiment in this repo); graduation into framework core is deferred
to [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md). Complements `rv:recap` (project
intent) — this is the *portable process* counterpart.

## Main flow

1. `rv:playbook [name]` (or "distill this into a playbook" / "lưu quy trình này thành playbook")
   → detect the `docs/` root; ensure `docs/playbooks/` exists (create if missing).
2. **Identify the process.** Scan the conversation for the sequence of phases that actually
   produced value (the workflow the session walked). Separate the *repeatable process* from
   *project-specifics* (names, values, one-off detours).
3. **Supplement from `docs/`.** For any step the conversation covers thinly, pull detail from
   existing `docs/wiki/`, `docs/plans/`, `docs/decisions/` to complete it; note which docs informed
   each step so the trace is auditable.
4. **Distill & generalize.** Abstract project-specifics into named placeholders/parameters; for
   each step record: purpose, inputs, actions, output (+ which riverflow artifact it produces),
   decision points, and pitfalls/lessons observed in the real run.
5. **Write** `docs/playbooks/<NNNN>-<slug>.md` from `docs/framework/templates/playbook.md`. Auto-write a best
   recommendation (rv:recap ethos), then let the human adjust — do not interrogate first.
6. **Hand off.** State where it was saved and how to reuse it: copy the file into another riverflow
   project; the riverflow scaffolding there makes regenerating each step's artifact easy.

## Edge cases & error states

| Situation | Expected behavior |
| --- | --- |
| Conversation has no clear repeatable process (one tiny patch) | say so, don't fabricate a playbook |
| Conversation walked several independent workflows | default to one end-to-end playbook; split only when the workflows are clearly separate, and say which split was chosen |
| A step is thin in chat but documented in `docs/` | fill it from the doc and cite it (step 3) |
| A playbook on the same topic already exists | update/extend it, don't shadow with a duplicate |
| No `docs/` layout in the repo | ask whether to initialize riverflow / create `docs/playbooks/` first |

## Constraints

- Pure Markdown + conversation; no tooling (riverflow's maturity stance).
- Output is **descriptive only** — a doc you read and follow, never an executable `SKILL.md`
  (per the brainstorm choice; the doc→skill graduation is a separate deferred idea).
- Standalone-only for now: **must not** touch framework distribution surfaces (README skills tree,
  `rv:update-version` copy list, `docs/framework/03-artifacts.md` catalog) — that wiring is the
  graduation step, backlogged.
- Generalization must keep a usable concrete example inline so the abstraction stays legible.

## Technical design

Two new files:

- `.claude/skills/rv:playbook/SKILL.md` — the procedure (detect → identify process → supplement
  from docs → distill/generalize → write → hand off, plus a Boundaries section drawing the line vs
  `rv:recap`).
- `docs/framework/templates/playbook.md` — the playbook template (see sections below).

Output location: `docs/playbooks/<NNNN>-<slug>.md`, numbering increasing within the dir per
`04-conventions.md`.

**Playbook template sections:** frontmatter (`name`, `when_to_use`, `source` = session/project,
`inputs`, `produces` = artifact types touched, `maturity` = `draft` / `proven xN`); Goal; When to
use / preconditions; Inputs needed; **Steps** (ordered — each: Purpose / Inputs / Do / Output (+
riverflow artifact) / Decision points); Pitfalls & lessons (from the real run); Parameters to fill
per project; Related riverflow artifacts.

**No framework-doc or distribution changes** in this plan (standalone-first). The skill reads the
existing `docs/` tree; it adds a `docs/playbooks/` directory on first use.

## Decisions

- **Named `playbook`, not `workflow`** — "workflow" clashes with Claude Code's Workflow
  orchestration concept; "playbook" reads as *read-and-follow procedure*, which is exactly the
  descriptive-doc form chosen.
- **Descriptive doc only** (no executable skill output) — chosen in brainstorm; keeps the artifact
  simple and avoids premature codification. The doc→skill graduation is parked as a future idea.
- **Standalone skill first, core later** — de-risk before changing the framework; graduation
  (template into catalog, README/update-version wiring, an ADR for the new artifact category)
  is [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md).
- **Store in-project** (`docs/playbooks/`), reuse by copy — keeps the artifact living with the
  session it came from (riverflow principle); a central library was rejected as out of framework
  scope.
- **Auto-write then adjust** (matches `rv:recap`) rather than an interactive interview.
- **One playbook = one end-to-end workflow** by default; split only for clearly independent flows.

> Promotion note: the choice to treat *playbook* as a portable artifact category will constrain the
> graduation work — kept local here while experimental, to be promoted to an ADR at graduation.

## Implementation steps

- [x] Write `docs/framework/templates/playbook.md`
- [x] Write `.claude/skills/rv:playbook/SKILL.md` (procedure + boundaries vs `rv:recap`)
- [x] Create `docs/backlogs/0003-rv-playbook-graduate-core.md` (graduation: ADR + catalog + wiring)
- [x] This plan + a worklog
- [x] Smoke-test: run `rv:playbook` against this very conversation → `docs/playbooks/0001-author-a-riverflow-skill.md`

## Acceptance criteria

- [x] `rv:playbook` exists as a standalone skill that produces a `docs/playbooks/<NNNN>-<slug>.md`
      from the current conversation, supplementing thin steps from existing `docs/`
- [x] The skill touches **no** framework distribution surface (README / `rv:update-version` /
      `03-artifacts.md`) — confirmed by grep
- [x] The output playbook is project-agnostic (placeholders for specifics) yet keeps a concrete
      example, and is copy-ready for another riverflow project

## Proof

| Tier | Needed? | Evidence |
| --- | --- | --- |
| Unit | no | docs/skill-only change |
| Integration | yes | ✅ `grep` over README / `docs/framework` / `rv:update-version` / `rv:release` → no `playbook` matches (standalone-first confirmed) |
| E2E | yes | ✅ `rv:playbook` run distilled this session → `docs/playbooks/0001-author-a-riverflow-skill.md`; skill registered in the session skill list |

## Out of scope

- Wiring the playbook into framework core (catalog, README, `rv:update-version`, an ADR) — that is
  [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md).
- Generating an executable skill from a playbook (the doc→skill graduation).
- A central / global playbook library outside the project.
