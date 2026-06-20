---
name: rv:playbook
description: Distill the reusable process the current conversation followed into a portable playbook — a read-and-follow Markdown file in docs/playbooks/ that can be copied into another riverflow project and re-run there. Unlike rv:recap (which captures project-specific intent — what/why — as plan/ADR/wiki), this captures the how: the repeatable sequence of steps (e.g. elicit BRD → draft design → …), generalized with placeholders for project-specifics. Because the session runs on riverflow, most steps and their per-step artifacts are recoverable straight from the conversation; where a step is thin, the skill supplements from existing docs/ (wiki/plans/ADRs). Canonical trigger is the prefix "rv:playbook" optionally followed by a name (e.g. "rv:playbook brd-to-design", "/rv:playbook"). Also fires on natural phrasing like "distill this into a playbook", "turn this workflow into a reusable doc", "lưu quy trình này thành playbook". Auto-writes a best-recommendation playbook immediately (without asking first); the user adjusts afterward. Use when the user wants to capture the WORKFLOW of a session as something reusable across projects — NOT for capturing this project's decisions (that is rv:recap).
---

# rv:playbook

Turns the **reusable process** a conversation walked into a **portable playbook**: a descriptive
`docs/playbooks/<NNNN>-<slug>.md` you read and follow, written to be copied into *another* riverflow
project and re-run there. It **auto-writes a best recommendation immediately** (rv:recap ethos), but
selectively — if the user wants it different, they say so and you adjust afterward.

Source framework: the riverflow repo (see `docs/framework/` and `docs/framework/templates/`).
riverflow principle here: *a process worth repeating should leave a readable artifact, not live only
in one chat session.*

## Why this is NOT rv:recap

They look similar ("read the conversation → write an artifact") but capture opposite axes:

| | `rv:recap` | `rv:playbook` |
| --- | --- | --- |
| Captures | project **intent** — what/why | portable **process** — how |
| Artifacts | plan / story / ADR / wiki / worklog | one `docs/playbooks/<NNNN>-<slug>.md` |
| Scope | this product, this project | reusable across projects (copy to reuse) |
| Lifespan | frozen record of one change | living recipe, bumped as it's reused |

At session end you may run **both**: recap the intent here, then playbook the process for next time.

## When to use

Canonical trigger: the prefix **`rv:playbook [name]`**. Also fires on "distill this into a
playbook", "turn this workflow into a reusable doc", "lưu quy trình này thành playbook". Do NOT run
automatically — only when triggered.

Do **not** use for: capturing this project's decisions (that is `rv:recap`); a one-off tiny patch
that has no repeatable process (say so, don't fabricate one).

## Procedure

### 1. Detect layout & target
Find the `docs/` root (standard riverflow: `docs/plans/`, `docs/wiki/`, `docs/decisions/`; embedded:
root-level dirs). Ensure `docs/playbooks/` exists — create it if missing. No `docs/` layout at all →
ask whether to initialize riverflow / create `docs/playbooks/` before continuing.

### 2. Identify the process
Scan the conversation for the **sequence of phases that actually produced value** — the workflow the
session walked (e.g. *elicit BRD → draft design → write plan → implement*). Separate the
**repeatable process** (worth keeping) from **project-specifics** (names, values, one-off detours
that won't recur). If the session walked several clearly independent workflows, default to **one
end-to-end playbook** and split only when they're genuinely separate — say which split you chose.

If there is no repeatable process (the session was a single tiny patch), stop and say so rather than
inventing ceremony.

### 3. Supplement from `docs/`
For any step the conversation covers thinly, pull the missing detail from existing
`docs/wiki/<topic>.md`, `docs/plans/`, and `docs/decisions/`. This is the riverflow advantage — the
scaffolding already holds the detail the chat skipped. **Cite** which doc informed each filled step
(in the step or in Pitfalls/notes) so the trace stays auditable. Never invent steps that neither the
conversation nor the docs support.

### 4. Distill & generalize
Rewrite the process to be portable. For each step record: **purpose**, **inputs**, **what you do**,
**output** (+ which riverflow artifact it produces, if any), and **decision points**. Abstract
project-specifics into named placeholders (`<...>`), but **keep a concrete example inline** (in
"Parameters to fill per project", with the real value from this session) so the abstraction stays
legible. Capture the **pitfalls/lessons** actually observed in this run — that is what makes the
playbook better than a generic checklist.

Watch the abstraction level: too generic is useless, too specific won't transfer. The test: could
someone in a *different* project follow this and produce the same kind of outcome?

### 5. Write the playbook
Write `docs/playbooks/<NNNN>-<slug>.md` (next number in the dir, `04-conventions.md`) from
`docs/framework/templates/playbook.md`. Fill the frontmatter: `name`, `when_to_use`, `source` (this
session/project), `inputs`, `produces` (artifact types touched), `maturity: draft`. If a playbook on
the same topic already exists, **update/extend it** and bump its `maturity` — do not shadow it with
a duplicate. Auto-write your best version; do not interview the user first.

### 6. Hand off
Say where it was saved and how to reuse it: **copy the file into another riverflow project and follow
it** — the riverflow scaffolding there makes regenerating each step's artifact straightforward.
Invite adjustments. If reuse becomes frequent, note that graduating it (proven playbook → executable
skill) is a possible next step — but that is out of this skill's scope.

## Boundaries

- **Descriptive doc only.** Output is a playbook you read — never an executable `SKILL.md`. (The
  doc→skill graduation is a separate, deferred idea.)
- **Standalone, not framework core (yet).** Do **not** touch framework distribution surfaces —
  README skills tree, `rv:update-version` copy list, `docs/framework/03-artifacts.md` catalog. That
  wiring is the graduation step and is backlogged.
- **Don't fabricate.** No repeatable process → no playbook. No supporting evidence in chat or docs →
  the step doesn't go in.
- **Generalize, but keep one real example.** A placeholder with no concrete anchor is unreadable.
- **Stay distinct from `rv:recap`** — process (how, portable), not intent (what/why, project-bound).
