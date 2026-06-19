---
updated: 2026-06-19
progress: "4/6 themes shipped"
---

# Roadmap: riverflow

> The forward-looking page for the riverflow framework itself — dogfood for the `roadmap` artifact
> type ([ADR-0005](../decisions/0005-product-roadmap-forward-layer.md)). Overwritten in place as
> themes ship.

## North star

A collaboration framework where every important decision and requirement leaves a durable artifact
readable by both humans and agents — so a fresh agent rebuilds full context from `docs/` alone, and
a product keeps moving without context living only in chat. Complete on **two axes**: present state
(where we are) **and** forward direction (where we're going), at every altitude (change → topic →
product).

## Now / Next / Later

### Now
- **Forward-looking product layer** — in-progress — ← [plan-0004](../plans/0004-product-roadmap-forward-layer.md), [ADR-0005](../decisions/0005-product-roadmap-forward-layer.md)
  The `roadmap` type — closes the last empty cell (product scope, living, future).

### Next
- **Roadmap automation** — planned — ← [backlog-0002](../backlogs/0002-rv-roadmap-skill.md)
  An `rv:roadmap` skill that refreshes theme status and refills the backlog from the gap.

### Later
- **1.0 stabilization** — planned
  Freeze the artifact catalog + conventions; pre-1.0 breaking-in-MINOR ends.

## Shipped

- **Artifact catalog consolidated around `plan`** — shipped — ← [plan-0001](../plans/0001-plan-artifact-consolidation.md), [ADR-0004](../decisions/0004-plan-artifact-consolidation.md)
- **Wiki as-built layer** (present-state mirror, per topic) — shipped — ← [ADR-0002](../decisions/0002-llm-wiki-as-built-layer.md)
- **Self-versioning + update check** — shipped — ← [ADR-0003](../decisions/0003-self-versioning-and-update-check.md)
- **Consumer skills** (`rv:recap`, `rv:brainstorm`, `rv:update-version`) — shipped — ← [plan-0002](../plans/0002-rv-brainstorm-skill.md)

## Gap-to-vision

The catalog now covers present state at change/topic level (plan, wiki) and was missing the
forward, product-level mirror — being added now (Now/theme above). After it ships, the next gap is
**automation** (keeping the roadmap fresh by hand is the manual step backlog-0002 targets), then
**1.0** to stop breaking conventions under MINOR. Diff this section against `docs/wiki/` when the
backlog runs dry.
