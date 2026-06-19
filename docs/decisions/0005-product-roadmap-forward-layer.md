# ADR-0005: `roadmap` — the per-product forward-looking layer

- **Status:** accepted
- **Date:** 2026-06-19
- **Decided by:** philoha
- **Related:** [03-artifacts](../framework/03-artifacts.md), [02-lifecycle](../framework/02-lifecycle.md), [04-conventions](../framework/04-conventions.md), [06-intake](../framework/06-intake.md), [ADR-0002](0002-llm-wiki-as-built-layer.md), [plan-0004](../plans/0004-product-roadmap-forward-layer.md)

> An ADR is for a choice that **outlives one change** — this one adds an artifact type to the
> catalog, so every future product cycle can rest on it.

## Context

riverflow has no artifact answering **"where is the product heading, and how far along are we
toward it?"** Mapping the catalog on two axes — *scope* (change → topic → product) × *time
direction* (frozen / living-present / backward) — leaves exactly one cell empty:

| | Frozen (point in time) | Living (overwritten) | Backward |
| --- | --- | --- | --- |
| **Change** | plan, ADR | — | worklog |
| **Topic** | — | **wiki** (as-built) | — |
| **Product** | **product-brief** | ⬅️ **empty** | retro |

- `product-brief` — product-scope, but **frozen once approved** (why we started); it cannot track
  progress against a moving destination.
- `wiki` — a living mirror, but of **present state per capability** (where we *are*), not of
  direction.
- `retro` — backward-looking, per milestone.
- `backlog` — a flat bag of deferred items with no north-star to measure against.

The same gap shows in the **loop**. `02-lifecycle` says discovery *pulls from* the backlog, and
the backlog is filled from worklog "still open" items + surfaced requests — i.e. **bottom-up
only**. There is no **top-down** feed (vision → themes → backlog). When the backlog runs dry,
nothing regenerates strategic work, and there is no way to ask *"is the vision met yet?"* without
reading the code — the product-level twin of the very question intake forces at change level.

This is the same shape of gap ADR-0002 closed for the present (it added `wiki`), one altitude up
and pointed at the future instead.

## Options

### Option A — Make `product-brief` a living document
Flip the brief to track progress and refresh it in place.
- Pros: no new artifact type; reuses an existing product-scope doc.
- Cons: forces the brief to carry **two roles** (frozen founding intent + a living progress
  tracker) → **wrong grain**, exactly the failure ADR-0002 rejected for spec. The brief's value is
  that it is frozen ("what we committed to at the start"); making it living destroys that trace.

### Option B — Add a per-product living `roadmap`, overwritten in place *(chosen)*
One page `docs/roadmap/<product>.md`: a stable north-star, Now/Next/Later themes with status, and a
gap-to-vision read. Living, refreshed as themes ship. `product-brief` keeps its frozen role.
- Pros: clean role separation — *scope* (one topic ↔ one product) and *answer* (present state ↔
  forward direction + progress). Becomes the **top-down source that refills the backlog**. Mirrors
  the wiki pattern (living, slug-named, freshness frontmatter, updated at the ship gate).
- Cons: a new artifact type (touches the minimalism principle); the classic roadmap doc-rot risk.

### Option C — No new type; infer direction from backlog + retro
- Pros: zero new ceremony.
- Cons: the backlog is flat — you cannot answer "how far to the vision" without reading every item
  and the code; there is no home for the north-star; the top-down refill loop stays missing.

## Decision

Chose **Option B (per-product `roadmap`)** because it is the **only artifact type that answers
"where the product is heading and how far along we are"** — and it *keeps `product-brief` frozen*
rather than overloading it. The three product-touching docs now separate cleanly, by scope and
time direction, not by metaphor:

| | product-brief | roadmap | wiki |
| --- | --- | --- | --- |
| Scope | one product/feature | one product | one topic/capability |
| Answers | why we started (problem, goals, success) | where we're going + how far | how it works right now |
| Lifecycle | frozen after approve | living, overwritten | living, overwritten |

The `roadmap` is the top-down counterpart to the backlog's bottom-up: discovery diffs the roadmap
against the wiki (as-built) to **refill the backlog** when it runs dry or when the vision changes.

**Optional by scale.** Like a story is optional by lane, a roadmap is optional by scale: a
single-feature product is served by its `product-brief`; a product spanning multiple themes across
multiple cycles needs the roadmap. Ceremony stays proportional.

## Consequences

- Positive: a product gets **one forward-looking recall entry point**; "is the vision met yet?"
  becomes answerable by rolling up theme status, no code read required; the backlog finally has a
  top-down source so the product keeps moving when bottom-up work dries up.
- Trade-offs we accept: one more artifact type; **doc-rot** treated with the same two mechanisms as
  wiki — (a) **forced update at the Ship gate** when a theme's status changes, (b) **fresh
  frontmatter** (`updated`, `progress`) signalling whether the page is still trustworthy.
- Naming: slug, not `NNNN` (`docs/roadmap/<product>.md`) — looked up by product, not by timeline;
  stored in its **own** `docs/roadmap/` dir, **not** `docs/plans/` (which is the frozen-intent
  zone — a living doc must not live there).
- Traceability: a plan gains a `Serves:` back-link to the roadmap theme it advances; the roadmap
  holds theme status as the single source of truth, so the rollup is read in one place.
- Lifecycle: a new top-down arrow — `roadmap → backlog → story/plan → ship → refresh roadmap
  progress`. Phase 1 (Discovery) and phase 6 (Ship) both reference it.
- What it opens up: if a portfolio view across products is ever needed, generate it from each
  roadmap's `progress`/theme frontmatter without changing storage.
