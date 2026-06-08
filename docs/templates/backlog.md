---
status: open          # open | promoted | dropped
lane_est: normal      # estimated lane when picked up: tiny | normal | high-risk
source: <where from>  # the worklog/story/ADR/conversation that produced this item, e.g. 2026-06-07-... | story-0003
created: YYYY-MM-DD
promoted_to: -        # when status=promoted: point to the real artifact, e.g. story-0004 | spec-0002 | task
---

# Backlog: <short title of the future work>

> Work you have **decided to defer**, not drop — not now, but worth keeping so a later session
> doesn't have to rethink it. When picked up, it "graduates" into a story/spec/task:
> change `status: promoted`, fill `promoted_to`, don't delete the file.

## What

One or two sentences: what to do, for whom, the expected result.

## Why deferred

Why not now (not up yet, waiting on a dependency, not enough signal, outside current scope).

## Pick-up trigger

The condition that makes this worth pulling out of the backlog: "when X happens", "after spec-0002 is done",
"if ≥N people ask". No clear trigger → most likely it should be `dropped` rather than kept.

## Notes

Context, related links, a sketch of the approach if you've thought of one.
