# ADR-0001: Backlog as per-item files in `docs/backlogs/`

- **Status:** accepted
- **Date:** 2026-06-07
- **Decided by:** philoha
- **Related:** [03-artifacts](../framework/03-artifacts.md), [04-conventions](../framework/04-conventions.md), [06-intake](../framework/06-intake.md), [templates/backlog.md](../framework/templates/backlog.md)

## Context

riverflow had no "home" for **deliberately deferred** work: ideas that surface mid-session, the
"still open" items of a worklog, surfaced-but-not-yet-up requests. These either drift away or get
crammed into the work at hand. We need a `backlog` artifact. The question: in what shape do we store it?

## Options

### Option A — One file per item, `<NNNN>-<slug>.md`
- Pros: consistent with ADR/story (numbered, cross-linkable, not deleted on promoted/dropped); each item "graduates" cleanly into a story/spec/task via `promoted_to`; greppable frontmatter (`status`, `lane_est`).
- Cons: more files for something that is essentially "a list".

### Option B — A single rolling list file `backlog.md`
- Pros: fewer files, less friction, fast whole-picture scan.
- Cons: hard to cross-link each item; balloons easily; doesn't fit riverflow's `<NNNN>-<slug>` convention; promoted/dropped history gets muddled in one file.

## Decision

Chose **Option A (per-item files)** because it folds into riverflow's existing artifact
conventions — numbering, keep-the-trace-don't-delete, cross-linking — and makes "graduate into a
story" a clear operation instead of editing one row in a table.

## Consequences

- Positive: backlog items share the same mental model as ADR/story; a later session can grep `status: open` for the queue; promote/drop leaves a trace.
- Trade-offs we accept: many small files; you need the discipline to `dropped` so `docs/backlogs/` doesn't become a junk drawer (the "no clear trigger → dropped" rule is recorded in 03-artifacts).
- What it opens up: if a "queue overview table" is ever needed, generate an index from the frontmatter without changing the storage structure.
