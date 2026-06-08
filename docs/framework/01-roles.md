# 01 — Roles & Boundaries

riverflow treats humans and agents as two collaborating roles. Each role owns different things. Clear boundaries let the agent know when it may decide on its own and when it must stop and ask.

## The human owns

- **Intent.** Who this product serves, what problem it solves, what success looks like.
- **Priority.** What to do first, what to drop.
- **Irreversible decisions.** Choosing a platform, changing the data model, committing a public API, spending money, touching real data or external systems.
- **Final approval.** Accepting an ADR, accepting a spec, merging a large change.

## The agent owns

- **Execution.** Writing code, writing tests, refactoring, scaffolding, drafting documents.
- **Proposals.** Offering options with their trade-offs; suggesting an ADR when it sees a big choice being made without one being recorded.
- **Spotting gaps.** "This requirement has no spec", "this decision conflicts with ADR-0007".
- **Maintaining artifacts.** Creating and updating worklogs, drafting user stories, writing ADRs at the human's direction.

## The grey zone: agent proposes, human approves

For **easily reversible** things (naming a variable, splitting a file, choosing a small internal library), the agent just does it and reports back.

For things that are **hard to reverse or expensive to undo**, the agent **stops and asks**, or drafts an ADR in `proposed` state for the human to approve:

- Changing the schema of data that already holds real data.
- Adding/changing a public API or a contract between services.
- Choosing a framework / platform / vendor.
- Any action that touches production data, sends data outward, or deletes/overwrites something the agent did not create.

## Handoff rules

- When the agent finishes a chunk of work → leave a `worklog`: what was done, why, what is still open. The human (or a later agent session) reads the worklog and continues.
- When the human assigns new work → point to the relevant story/spec, or ask the agent to create one if it doesn't exist.
- When the artifact and the conversation disagree → the artifact wins, unless the human actively updates the artifact.

## One line to remember

> The human owns *why* and *should we*. The agent owns *how* and *go do it*. Both own *did we write it down*.
