# 04 — Conventions

Conventions for naming, storage, and the handoff protocol. Goal: anyone (human or agent) can guess where a file lives and what its name means.

## Storage location

| Artifact | Directory |
| --- | --- |
| Decision (ADR) | `docs/decisions/` |
| User Story | `docs/stories/` |
| Plan / Brief | `docs/plans/` |
| Wiki | `docs/wiki/` |
| Worklog / Retro | `docs/worklogs/` |
| Backlog | `docs/backlogs/` |

> No `task` directory: a plan's **Implementation steps** checklist is the work breakdown; external tracking lives in Jira, linked from the plan.

## File naming

Format: `<NNNN>-<slug>.md`

- `NNNN` — a four-digit sequence number, increasing **within each directory** (`0001`, `0002`, …). Numbers are never reused, even when an artifact is `superseded`.
- `slug` — kebab-case, short, descriptive. English (or another language with correct orthography — never a lossy unaccented form).

Examples:
```
docs/decisions/0001-sqlite-over-postgres.md
docs/decisions/0002-public-api-versioning.md
docs/stories/0001-login-with-magic-link.md
docs/plans/0001-product-brief.md
docs/plans/0002-magic-link-auth.md
docs/worklogs/2026-05-30-scaffold-auth.md
```

> Worklogs use a date prefix `YYYY-MM-DD-<slug>.md` instead of a sequence number, because they read along a timeline.

> Backlog uses `<NNNN>-<slug>.md` like ADR/story (increasing within `docs/backlogs/`, never reused). A `promoted`/`dropped` item keeps its file — numbers are never reclaimed.

> Wiki uses `<topic-slug>.md` (`docs/wiki/rbac.md`), **not** numbered — it is looked up by topic, not read by timeline or creation order. One topic, one page, overwritten in place. The freshness signal lives in the frontmatter (`updated`, `verified_against_code`).

## ADR status

Each ADR has a `Status:` line at the top:

- `proposed` — proposed by agent or human, awaiting a decision.
- `accepted` — decided, in effect.
- `superseded by 00NN` — replaced by a newer ADR. Keep the old file intact.

## Plan status

Each plan has a `Status:` line at the top — it doubles as the review gate:

- `draft` — being brainstormed/written.
- `approved` — a human reviewed the plan; implementation may start (required for lane `normal`+).
- `done` — implemented with proof. The plan **freezes** here; current state moves to the wiki.

## Cross-linking

- A plan links to the stories it implements and the ADRs it follows.
- A worklog links to the story/plan the session touched.
- When an ADR supersedes an old one, write `Supersedes: 00NN` on the new ADR and `Superseded by: 00NN` on the old one.

Use relative paths: `[ADR-0001](../decisions/0001-....md)`.

## Language

- English, or another language with **correct orthography** — never a lossy unaccented form in files.
- Reply to the user in the language they use.

## Worklog frontmatter

A worklog opens with a YAML frontmatter block so machines/agents can grep it:

```yaml
---
date: YYYY-MM-DD
who: agent | human | both
lane: tiny | normal | high-risk
input_type: new-brief | story-slice | change-request | initiative | maintenance | framework-improvement
outcome: completed | partial | blocked | failed
touched: [story-0001, plan-0001, ADR-0002]
files_changed: [path/to/file]
friction: none
---
```

`input_type` records the input type chosen at intake (`06-intake.md`) — so you can later grep
"what type did this work come **in** as → what result did it go **out** as". `friction` notes
briefly where things snagged (missing docs, unclear proof, a repeated manual step) — a signal for
later framework improvement. `tiny` needs only `date` + `outcome`.

## Handoff protocol (summary)

1. Pick up work → run intake (`06-intake.md`): classify the input + lane (`05-risk.md`), read the relevant `docs/decisions/` and `docs/plans/`, then read out the Output block.
2. A choice that outlives this change appears → create an ADR in `proposed` state; don't self-approve if it is hard to reverse. A choice local to the change → the plan's **Decisions** section.
3. `normal`+ → get the plan `approved` before implementing; fill the Proof table in the story/plan before calling it done.
4. Close a chunk of work → write a `worklog` (with frontmatter).
5. Artifact conflicts with the conversation → the artifact wins until the human updates it.

## Size

Keep artifacts short. Brief one page. ADR half a page. Story a few lines + acceptance criteria. If a file balloons, split it or ask whether it still answers exactly one question.
