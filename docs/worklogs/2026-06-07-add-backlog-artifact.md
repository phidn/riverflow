---
date: 2026-06-07
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [ADR-0001]
files_changed:
  - templates/backlog.md
  - docs/backlogs/.gitkeep
  - framework/02-lifecycle.md
  - framework/03-artifacts.md
  - framework/04-conventions.md
  - framework/06-intake.md
  - AGENTS.md
  - README.md
  - docs/decisions/0001-backlog-per-item-files.md
friction: none
---

# Worklog: 2026-06-07 — Add the `backlog` artifact

## Done

- Added a new artifact type `backlog`: the home for **deliberately deferred** work (not now but worth keeping).
- `templates/backlog.md` — frontmatter (`status` / `lane_est` / `source` / `created` / `promoted_to`) + the What / Why deferred / Pick-up trigger / Notes sections.
- Created `docs/backlogs/` (per-item files `<NNNN>-<slug>.md`).
- Wired it across the framework: the artifact table (03), storage + naming conventions (04), intake's emergence relief valve (06), the lifecycle loop (02), the "when to create which artifact" table in AGENTS.md, the directory tree + template catalog in README.
- Recorded [ADR-0001](../decisions/0001-backlog-per-item-files.md) locking in per-item files over a single rolling list file.

## In-place decisions

- Backlog is the **home of "still open"**: a worklog's "still open" items and surfaced-but-not-yet-up ideas land here instead of drifting away — which is why it is wired into both 06-intake (the relief valve) and 02-lifecycle (the next discovery cycle pulls from it).
- Keep the "no clear trigger → `dropped`" discipline so the folder doesn't become a junk drawer.

## Still open

- [ ] The `riverflow-capture` skill currently only writes ADR + worklog; it doesn't yet create a backlog item from a conversation's "still open". Consider extending it if the backlog gets real use.

## Next step

Try it out: next time there is deferred work, create `docs/backlogs/0001-<slug>.md` from the template and see whether the promote → story flow is smooth.
