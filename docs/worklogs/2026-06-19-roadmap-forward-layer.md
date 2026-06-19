---
date: 2026-06-19
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [plan-0004, ADR-0005, backlog-0002, roadmap-riverflow]
files_changed:
  - docs/decisions/0005-product-roadmap-forward-layer.md
  - docs/plans/0004-product-roadmap-forward-layer.md
  - docs/backlogs/0002-rv-roadmap-skill.md
  - docs/framework/templates/roadmap.md
  - docs/roadmap/riverflow.md
  - docs/framework/02-lifecycle.md
  - docs/framework/03-artifacts.md
  - docs/framework/04-conventions.md
  - docs/framework/06-intake.md
  - docs/framework/templates/plan.md
  - docs/framework/VERSION
  - docs/framework/CHANGELOG.md
  - AGENTS.md
friction: CLAUDE.md is a symlink → AGENTS.md (edit the real target); a pre-existing example link in 04-conventions.md (0001-....md) trips a naive link-checker — it is a placeholder, not real.
---

# Worklog — add the `roadmap` forward-looking product layer

## What

Added riverflow's 9th artifact type, `roadmap` — the living, forward-looking, product-level page
that was the one empty cell in the catalog (product scope × future direction). It mirrors the wiki
one altitude up: wiki = *where we are* (as-built), roadmap = *where we're going + how far*. It is
the top-down source that refills the backlog when the backlog runs dry or the vision changes.

Started as a "thinking/verify" question from the user (riverflow felt like it was missing a
vision/roadmap layer), ran `rv:brainstorm`, crystallized ADR-0005 + plan-0004, the user approved,
and implemented the whole plan in the same session.

## Why

The framework was strong bottom-up (emergence → backlog) but had no top-down strategy feed: when
the backlog emptied, nothing regenerated strategic work, and "is the vision met yet?" was
unanswerable without reading code. Same shape of gap ADR-0002 closed for present-state with the
wiki.

## Decisions (locked)

- Type name `roadmap` (not `vision`) — defining trait is living progress/gap tracking.
- Own dir `docs/roadmap/<product>.md`, slug-named (like wiki) — keeps the "every type has a
  `docs/<type>/` home" rule and avoids a migration when a 2nd product appears. The user pushed on
  "why a folder for one file"; kept the folder for convention uniformity + zero-migration growth.
- Optional by scale; plan `Serves:` back-link with the roadmap as single source of truth for theme
  status.

## Proof

Integration: link-check over all new/edited docs → no dangling `.md` links (one hit is the
pre-existing placeholder in `04-conventions.md`). `docs/roadmap/riverflow.md` dogfoods the template.

## Still open

- No wiki page created: the riverflow catalog's as-built lives in `03-artifacts.md` itself, so a
  separate `docs/wiki/` page would duplicate it. Noted, not a gap.
- `backlog-0002` (`rv:roadmap` skill to auto-refresh + refill backlog) deferred — pick up after the
  roadmap has been maintained by hand for ≥2 cycles.
- Release is staged in-repo (VERSION 0.7.0 + CHANGELOG). Not committed/pushed — awaiting the user.
