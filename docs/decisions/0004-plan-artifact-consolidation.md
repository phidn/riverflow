# ADR-0004: Consolidate spec + task into a single `plan` artifact

- **Status:** accepted
- **Date:** 2026-06-11
- **Decided by:** philo
- **Related:** [plan-0001](../plans/0001-plan-artifact-consolidation.md)

> An ADR is for a choice that **outlives one change** — this one redefines the framework's
> artifact catalog, so every future plan depends on it.

## Context

The actual working loop is *brainstorm → log a plan → review the plan → implement → recap into
`docs/`*. The framework's nouns didn't match that loop: a technical design had no single home —
its content was split across `spec` (behavior), `task` (steps), and ADR (choices) — and the word
"spec" misled people into treating a frozen-after-ship document as if it had to stay current.
Industry context (2026): spec-driven frameworks (GitHub Spec Kit, Amazon Kiro) converge on the
same loop and also name the technical document "plan", but keep spec/plan/tasks as three files
per feature.

## Options

### Option A — Keep spec + task + ADR as separate types
- Pros: matches Spec Kit/Kiro vocabulary; each file stays single-purpose.
- Cons: three files per change is ceremony for a solo/agent workflow; same grain and lifecycle
  (one change, frozen after ship) split across files; "spec" naming keeps misleading.

### Option B — Merge spec + task + ADR all into `plan`
- Pros: one home for everything about a change.
- Cons: ADRs are grained by *decision*, not by *change* — they outlive plans, carry a
  `proposed → accepted → superseded` chain, and are looked up by topic. Burying them in
  frozen plans loses the supersede chain and the lookup path.

### Option C — Merge spec + task into `plan`; keep ADR with a raised bar *(chosen)*
- Pros: one document per change (what + how + steps), named for what it is; decisions local to a
  change live inline in the plan's **Decisions** section; only choices that outlive the change
  get an ADR (with a promotion rule for misjudged ones). Story stays as the demand-side tracking
  unit, optional by lane.
- Cons: breaking rename for installed repos (`docs/specs/` → `docs/plans/`); two homes for
  decisions requires a clear boundary question.

## Decision

Chose **Option C** because **the plan and the spec/task share one grain and one lifecycle (one
change, frozen after ship) while ADRs do not — so merging the former loses nothing and merging
the latter loses the supersede chain and topic lookup**. The boundary question for inline vs ADR:
*"six months from now, working on something else, would anyone need to look this up?"*

Also locked in with this change: plan status `draft → approved → done` makes the human
plan-review gate explicit (lane `normal`+ requires `approved` before implementing), and
Implementation steps are a checkbox list so a later session can resume mid-plan.

## Consequences

- Positive: framework nouns match the real loop (plan mode, "review the plan"); one file to
  review per change; fewer, higher-signal ADRs.
- Trade-offs we accept: installed repos must migrate (`git mv docs/specs docs/plans`, template
  renames — see CHANGELOG 0.3.0); `product-brief` now lives in `docs/plans/` though it is not
  strictly a plan.
- What it opens up / closes off later: opens a future `rv:plan` skill (crystallize a plan from a
  brainstorm session — backlog-0001); closes the door on a separate `task` artifact type.
