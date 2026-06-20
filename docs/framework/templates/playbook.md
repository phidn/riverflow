---
name: <slug>                       # the workflow, e.g. brd-to-design, incident-postmortem
when_to_use: <one line>            # the trigger condition — when you'd reach for this playbook
source: <session / project>        # where it was distilled from, e.g. "riverflow / 2026-06-20 session"
inputs: []                         # what you must have before starting: [a stakeholder, a problem statement]
produces: []                       # riverflow artifact types this workflow yields: [plan, user-story, wiki]
maturity: draft                    # draft | proven xN  (bump each time it's reused successfully)
---

# Playbook: <workflow name>

> A **portable** how-to: it describes *how a kind of work is done*, generalized to be copied into
> another riverflow project and re-run there. Project-specifics are placeholders (`<...>`); the
> repeatable steps are the value. Not a record of one product (that is a plan/wiki) — a reusable
> process.

## Goal

One or two sentences: what outcome this workflow achieves, and for whom.

## When to use / preconditions

- Trigger: <when you'd start this workflow>
- Preconditions: <what must already be true / exist>

## Inputs needed

| Input | What it is | Where it comes from |
| --- | --- | --- |
| `<input>` | <description> | <source> |

## Steps

The ordered process. Each step: its purpose, what it consumes, what you do, what it produces (and
which riverflow artifact, if any), and where a decision branches.

### 1. <step name>

- **Purpose:** <why this step exists>
- **Inputs:** <what it consumes>
- **Do:** <the action — concrete enough to follow>
- **Output:** <result> → <riverflow artifact, e.g. `docs/plans/<NNNN>-*.md`, or "none">
- **Decision points:** <branch / when to skip / when to loop back>

### 2. <step name>

- **Purpose:**
- **Inputs:**
- **Do:**
- **Output:**
- **Decision points:**

## Pitfalls & lessons

What went wrong (or nearly did) in the real run this was distilled from — so the next run avoids it.

- <pitfall / lesson>

## Parameters to fill per project

The placeholders to swap when reusing this playbook elsewhere.

| Placeholder | Meaning | Example (from source) |
| --- | --- | --- |
| `<...>` | <what it stands for> | <concrete value in the original session> |

## Related riverflow artifacts

Artifact types this workflow touches, so a new project knows what scaffolding it will fill.

- <artifact type> — <when in the flow it's produced>
