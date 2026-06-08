# 02 — Lifecycle

Work in riverflow flows through six phases. Each phase has a central question and produces a characteristic artifact type. The lifecycle is not strictly linear — it loops — but the artifact order is stable.

> **Before every phase: Intake.** Every request that leads to a code/artifact change passes through the intake gate — classify the input + risk, then read out an Output block that routes it into the right phase below. Intake creates no file; it is a forcing function against skipping the intent layer. → [06-intake.md](06-intake.md)

```
discovery → spec → decision → build → review → ship
    │         │        │         │        │       │
  brief    spec/    decision   task/   worklog   wiki/
          story     (ADR)     worklog          worklog/retro
```

## 1. Discovery — "What problem are we solving?"

Clarify the problem, the user, the goals, the non-goals, the success metrics.

- **Artifact:** `product-brief`
- **Human:** owns intent, locks in goals & non-goals.
- **Agent:** asks clarifying questions, drafts the brief, flags unverified assumptions.
- **Exit when:** there is a human-approved brief.

## 2. Spec — "How exactly does it work?"

Turn goals into user needs and a behavior spec.

- **Artifact:** `user-story`, `spec`
- **Human:** confirms it is the real need.
- **Agent:** drafts story + acceptance criteria, specs out the flows & edge cases.
- **Exit when:** there is a spec/story clear enough to start, with verifiable acceptance criteria.

## 3. Decision — "Which direction, and why?"

Whenever you face an important choice, record the choice and the reasoning.

- **Artifact:** `decision` (ADR)
- **Human:** locks in the hard-to-reverse decisions.
- **Agent:** lists options + trade-offs, proposes, drafts the ADR in `proposed` state.
- **Exit when:** the ADR is in `accepted` state.

> Decision is not a separate phase in time — it interleaves throughout build. Any time a big choice appears, stop and record an ADR.

## 4. Build — "Make it."

Execution: code, test, scaffold.

- **Artifact:** `task`, `worklog`
- **Before starting:** pick a lane (`05-risk.md`). The lane defines the required artifacts + how far proof must go.
- **Agent:** owns execution; writes a worklog for meaningful chunks of work.
- **Human:** unlocks hard-to-reverse decisions (high-risk / hard gate) when the agent stops to ask.
- **Exit when:** the spec's acceptance criteria are met.

## 5. Review — "Is it correct?"

Verify against the spec; hunt for bugs, trade-offs, technical debt.

- **Artifact:** `worklog` (the review result), update the `decision` if something emerges
- **Human:** final approval.
- **Agent:** runs tests, checks against acceptance criteria, fills the Proof table in the story/spec, reports deviations.
- **Exit when:** acceptance criteria are met, the Proof table is complete to the lane's depth, and the review is accepted.

## 6. Ship — "Release it and write it down."

Release and close the learning loop.

- **Artifact:** `wiki` (update the as-built), `worklog` (ship note), `retro` (for a milestone)
- **Agent:** before closing the session, **update `docs/wiki/<topic>.md`** for every capability this change touches — overwrite the current state, update `updated`/`verified_against_code`/`source_specs`. This is a **mandatory** step, not goodwill: a spec is the commitment of one change, the wiki is where current-state settles. If the topic has no page yet, create one from `docs/templates/wiki.md`.
- **Human + Agent:** write the retro together — what worked, what to fix next time.

## The loop

After ship, the lessons in `retro` feed back into the `discovery` of the next cycle. riverflow assumes continuous iteration, not a one-way waterfall.

Between cycles, **deliberately deferred** work does not vanish: the "still open" items in a worklog and the ideas-that-surfaced-but-aren't-up-yet land in `backlog` (`docs/backlogs/`). The next `discovery` cycle pulls from the backlog, picking items up into story/spec/task (`status: promoted`). → [03-artifacts.md](03-artifacts.md)
