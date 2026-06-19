---
status: open
lane_est: normal
source: plan-0004 (roadmap forward layer brainstorm)
created: 2026-06-19
promoted_to: -
---

# Backlog: `rv:roadmap` skill — refresh roadmap + refill backlog from the gap

## What

A skill that reads `docs/roadmap/<product>.md` + the wiki (as-built), computes the gap-to-vision,
refreshes theme status from the plans that `Serves:` each theme, and proposes backlog items when
the backlog has run dry or the vision changed. The automated twin of the manual discipline that
ADR-0005 / plan-0004 introduce.

## Why deferred

The artifact type comes first — a skill to maintain it only makes sense once the `roadmap` exists
and has been used by hand for a while. Premature automation would harden a workflow we haven't
felt yet.

## Pick-up trigger

After plan-0004 ships and the roadmap has been maintained manually for ≥2 cycles and the refresh /
backlog-refill step feels repetitive.

## Notes

Sits between `rv:brainstorm` (front of the loop) and `rv:recap` (back). Could fold the
backlog-refill step into discovery. Related: [plan-0004](../plans/0004-product-roadmap-forward-layer.md),
[ADR-0005](../decisions/0005-product-roadmap-forward-layer.md).
