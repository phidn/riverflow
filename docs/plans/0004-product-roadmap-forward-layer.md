# Plan-0004: Add the `roadmap` forward-looking product layer

- **Status:** done
- **Related brief:** —
- **Serves:** [docs/roadmap/riverflow.md](../roadmap/riverflow.md) → "Forward-looking product layer"
- **Related stories:** —
- **Related ADRs:** [ADR-0005](../decisions/0005-product-roadmap-forward-layer.md)

> Status is the review gate: `draft` while brainstorming/writing, `approved` once a human has
> reviewed the plan, `done` when implemented with proof.

## Summary

riverflow has no living, forward-looking, product-level artifact — the cell "product scope, living,
future direction" is empty (see [ADR-0005](../decisions/0005-product-roadmap-forward-layer.md)). This
change adds a `roadmap` artifact type: a per-product page (`docs/roadmap/<product>.md`) holding a
stable north-star, Now/Next/Later themes with status, and a gap-to-vision read. It is the top-down
source that refills the backlog and lets anyone answer "is the vision met yet?" without reading the
code. The change is documentation-only (framework + a new template + a self-hosted sample).

## Main flow

1. A product spanning multiple themes/cycles creates `docs/roadmap/<product>.md` from the new
   template (optional-by-scale — a single-feature product stays on its `product-brief`).
2. Discovery reads the roadmap, diffs it against the wiki (as-built), and **refills the backlog**
   from the gap (or when the vision itself changes).
3. A backlog item graduates into a story/plan as today; the plan adds a `Serves:` line back to the
   roadmap theme it advances.
4. At Ship, when a theme's status changes (a plan under it ships), the agent refreshes the
   roadmap's theme status + `updated`/`progress` frontmatter — same forced-update discipline as
   the wiki.

## Edge cases & error states

| Situation | Expected behavior |
| --- | --- |
| Single-feature product, no themes | No roadmap; `product-brief` suffices (optional-by-scale). |
| Multiple products in one repo | One file per product under `docs/roadmap/` (slug-named). |
| Roadmap goes stale | `updated`/`progress` frontmatter signals staleness, like wiki freshness. |
| Plan advances no roadmap theme | `Serves:` line omitted / `—` (tiny or infra-only work). |

## Constraints

- Stay inside the minimalism principle: roadmap is thin in detail (north-star + themes + gap), with
  detail pushed down to plans. Guard against it becoming a monolith.
- No breaking change to existing artifact instances; only additions + cross-references.
- Consistent with the wiki precedent (ADR-0002): living, slug-named, freshness frontmatter,
  forced update at ship.

## Technical design

Documentation changes, in the riverflow core (`docs/framework/`):

1. **New template** `docs/framework/templates/roadmap.md` — frontmatter (`updated`, `progress`) +
   sections: North star · Now / Next / Later (theme + status: `planned | in-progress | shipped`) ·
   Gap-to-vision (the backlog-refill source).
2. **`03-artifacts.md`** — add `Roadmap` to the summary table and a detail subsection; add the
   three-way boundary table (product-brief ↔ roadmap ↔ wiki) from ADR-0005; extend "How to choose
   an artifact" with the brief-vs-roadmap distinction.
3. **`02-lifecycle.md`** — add the top-down arrow `roadmap → backlog → story/plan → ship → refresh
   roadmap`. Reference roadmap in phase 1 (Discovery pulls/refills backlog from it) and phase 6
   (Ship refreshes theme status). Note it sits above the existing loop, not inside a single phase.
4. **`04-conventions.md`** — storage row (`Roadmap | docs/roadmap/`) + naming note (slug, not
   `NNNN`, like wiki); add `Serves:` to the plan cross-linking rules.
5. **`06-intake.md`** — the Output block reads a `Roadmap:` line (which product roadmap this work
   serves, or "n/a"); note that a backlog that has run dry is refilled from the roadmap, not invented.
6. **`templates/plan.md`** — add a `Serves:` frontmatter line (→ roadmap theme).
7. **`CLAUDE.md` + `00-overview.md`** — add roadmap to the "which artifact" table and the layer list.
8. **Self-host a sample** — create `docs/roadmap/riverflow.md` for the riverflow framework itself
   (themes: artifact catalog, skills, versioning…) as the worked example + dogfood.
9. **Release** — `rv:release` MINOR bump (new artifact type, backward compatible) + CHANGELOG entry.

## Decisions

Locked here (the cross-change ones are promoted to [ADR-0005](../decisions/0005-product-roadmap-forward-layer.md)):

- Type name `roadmap` over `vision` — the artifact's defining trait is the living progress/gap
  tracking, which "roadmap" names better; "Vision" is a section (north-star) inside it.
- Theme status enum `planned | in-progress | shipped` — mirrors plan/ADR status vocabulary.
- The roadmap holds theme status (single source of truth); plans only back-link via `Serves:` —
  avoids a two-place sync.

## Implementation steps

- [x] Flip ADR-0005 `proposed → accepted` (human) before implementing.
- [x] Write `docs/framework/templates/roadmap.md`.
- [x] Update `03-artifacts.md` (table + detail + boundary table + choose-an-artifact).
- [x] Update `02-lifecycle.md` (top-down arrow + phase 1 & 6 references).
- [x] Update `04-conventions.md` (storage + naming + `Serves:` cross-link).
- [x] Update `06-intake.md` (Output block `Roadmap:` line + backlog-refill note).
- [x] Update `templates/plan.md` (`Serves:` frontmatter line) + back-link plan-0004 itself.
- [x] Update `CLAUDE.md`/`AGENTS.md` table. (`00-overview.md` left unchanged — it has no artifact list to amend; the catalog lives in `03-artifacts.md`.)
- [x] Create `docs/roadmap/riverflow.md` self-hosted sample.
- [x] VERSION → 0.7.0 (MINOR) + CHANGELOG entry.

## Acceptance criteria

- [x] A new `roadmap` template exists and is referenced from `03-artifacts.md`.
- [x] The framework docs (02/03/04/06) + CLAUDE.md cross-reference the type consistently (no dangling links).
- [x] `templates/plan.md` carries a `Serves:` line.
- [x] `docs/roadmap/riverflow.md` exists and reads cleanly as a worked example.
- [x] VERSION + CHANGELOG bumped one MINOR.

## Proof

| Tier | Needed? | Evidence |
| --- | --- | --- |
| Unit | no | — |
| Integration | yes | link-check script over all new/edited docs → no dangling `.md` links (the one hit is a pre-existing example placeholder in `04-conventions.md`); `docs/roadmap/riverflow.md` validates the template end-to-end |
| E2E | no | — |

## Out of scope

- Any portfolio/multi-product rollup view (left as a future possibility in ADR-0005).
- Tooling/skills automation (e.g. an `rv:roadmap` skill) — separate backlog item if wanted later.
- Changes to product-brief content (it stays frozen-intent; only cross-references added).
