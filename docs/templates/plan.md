# Plan-NNNN: <change name>

- **Status:** draft | approved | done
- **Related brief:** <link>
- **Related stories:** <link, link>
- **Related ADRs:** <link>

> Status is the review gate: `draft` while brainstorming/writing, `approved` once a human has
> reviewed the plan (required before implementation for lane `normal`+), `done` when implemented
> with proof. A `done` plan is **frozen** — it records the intent of this change at the time it
> was made; the current state of the capability lives in the wiki.

## Summary

One paragraph: what this change does and why it exists.

## Main flow

Describe the expected behavior of the happy path, step by step.

1.
2.
3.

## Edge cases & error states

| Situation | Expected behavior |
| --- | --- |
| <edge case> | <behavior> |
| <error> | <message / handling> |

## Constraints

Performance, security, compatibility, or technical limits to follow.

## Technical design

How the change is built: affected modules/files, data model or schema changes, migrations,
sequencing, rollback story. Enough for a reviewer to approve the direction **before any code is
written** — this section is what the `approved` gate reviews.

## Decisions

Local decisions made for this change — one line each: *chose X over Y because Z*.

> **Promotion rule:** a decision that will constrain changes **beyond this one** (a later plan
> would need to look it up) does not belong here — give it its own ADR in `docs/decisions/` and
> link it under **Related ADRs**. If an inline decision turns out to matter later, promote it to
> an ADR at that point.

- <decision>

## Implementation steps

The work breakdown, as a checklist — tick steps as they complete so a later session can resume
mid-plan. (Work tracked externally → link the Jira key here instead.)

- [ ] <step>
- [ ] <step>

## Acceptance criteria

- [ ] <verifiable>
- [ ] <verifiable>

## Proof

Verification tiers by lane (see [framework/05-risk.md](../framework/05-risk.md)).
The child stories of this plan track proof in detail; here record the minimum the plan demands.

| Tier | Needed? | Evidence |
| --- | --- | --- |
| Unit | yes / no | <link / command> |
| Integration | yes / no | <link / command> |
| E2E | yes / no | <link / command> |

## Out of scope

Things explicitly NOT in this plan.
