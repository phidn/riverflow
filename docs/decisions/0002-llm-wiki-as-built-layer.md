# ADR-0002: `wiki` — the per-topic as-built layer

- **Status:** accepted
- **Date:** 2026-06-07
- **Decided by:** philoha
- **Related:** [03-artifacts](../framework/03-artifacts.md), [02-lifecycle](../framework/02-lifecycle.md), [04-conventions](../framework/04-conventions.md), [06-intake](../framework/06-intake.md), [templates/wiki.md](../templates/wiki.md), [templates/spec.md](../templates/spec.md)

## Context

riverflow had no artifact answering **"how does capability X *currently* work in the system?"**
(e.g. after implementing RBAC, what do you open the repo and read to understand how RBAC
operates?). Looking at the existing catalog, each type answers a different question:

- `spec` — written in the future tense ("expected behavior"), grained **by a single change**;
  it self-generates small and numerous, and never self-merges into the topic-level picture.
- `worklog` — a log **per session**; a capability built over 5 sessions scatters its knowledge across 5 files.
- `decision` (ADR) — a **decision** slice, not a full description.

Phase 6 (Ship) currently only produces `worklog` + `retro` — it does **not lock in a capability
description**. That is a gap: riverflow is strong on *decisions* and *process*, weak on *the
durable end-state description of a capability*. The question: how do we close it?

## Options

### Option A — Make the spec a living document
Flip `spec.status → implemented` after build and update the spec in place to match the as-built.
- Pros: no new artifact type; reuses the existing `implemented` status.
- Cons: forces the spec to carry **two roles** (forecasting one change + the living handbook of a
  whole topic) → **wrong grain**. A spec is anchored to one change; "how the capability works" is
  anchored to a topic that accumulates across many specs. Small specs don't self-merge. The living
  role makes the spec rot.

### Option B — Add a per-topic `wiki`, overwritten in place
One page `docs/wiki/<topic>.md` per capability; each related ship overwrites it.
The spec **keeps** its one-change grain (content frozen after ship, like an ADR).
- Pros: clean role separation on two axes — *scope* (one change ↔ one topic) and *answer* (the
  commitment of one change ↔ the current state). `grep rbac` → one canonical page instead of
  stitching together 5 specs + 8 worklogs. Frees the spec from two roles.
- Cons: a 9th artifact type (touches the minimalism principle); the classic wiki doc-rot risk.

## Decision

Chose **Option B (per-topic `wiki`)** because it is the **only artifact type that answers "the
current state of a topic"** — and it *cleanly reassigns the spec's role* (the spec stops chasing
the present) rather than just stacking on top. The two types don't step on each other because
their **scope differs**, not because "this one is alive and that one is dead".

The boundary, locked (the canonical form, no metaphor):

| | Spec | Wiki |
| --- | --- | --- |
| Scope | one *change* | one *topic/capability* |
| Answers | "what does *this change* commit to, what proof passes?" | "how does *this capability* work right now?" |
| Lifecycle | content frozen after ship (keeps a trace, like an ADR) | living, overwritten in place on each related ship |

## Consequences

- Positive: a later session gets **one recall entry point** per topic; intake suggests reading
  `docs/wiki/<topic>.md` before editing. The spec gets leaner, free of the "alive or dead" tension.
- Trade-offs we accept: one more artifact type; **doc-rot** must be treated actively with two
  mechanisms — (a) **forced update at the Ship gate** (a step in phase 6, not left to goodwill),
  (b) **fresh frontmatter** (`updated`, `verified_against_code`, `source_specs`) so an agent knows
  whether the page is still trustworthy or has gone stale.
- Naming: the wiki uses a **topic slug** (`docs/wiki/rbac.md`), **not** an `NNNN` number — because
  it is looked up by topic, not read by timeline or creation order.
- What it opens up: if a "capability index" is ever needed, generate it from the `topic`/`updated`
  frontmatter without changing the storage structure.
