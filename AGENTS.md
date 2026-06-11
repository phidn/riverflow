# AGENTS.md — riverflow

Rules for agents working in a product that applies riverflow.

## Principles

- Every important decision and requirement must leave a readable artifact. Do not let context live only in a chat session.
- Every request that leads to a code/artifact change goes through **intake** (`docs/framework/06-intake.md`): classify the input + lane, then read out the Output block (`Lane / Type / Docs / Story / Proof`) before editing. The `Story` line forces a definite choice — point to an existing file, propose creating one, or say outright "not needed — tiny patch"; silence is not an option.
- A **second request touching the same topic** with no story/plan yet → propose an initiative (a thin plan + story). Ask yourself: "can I answer *is this done yet* without reading the code?" — if not, you need a story/plan.
- Before choosing a technical direction that has multiple options, check: is there a `decision` (ADR) already? If not: a choice local to this change → record it in the plan's **Decisions** section; a choice that outlives this change → create an ADR.
- Language: write artifacts in English. (If your project operates in another language, use it with correct orthography — never a lossy unaccented form.)

## When to create which artifact

| Situation | Artifact | Stored in |
| --- | --- | --- |
| A new product/feature goal | `product-brief` | `docs/plans/` |
| A user need to describe | `user-story` | `docs/stories/` |
| A change to design before implementing (technical design + steps) | `plan` | `docs/plans/` |
| Record how a shipped capability *currently* works (living as-built) | `wiki` | `docs/wiki/` |
| A technical/product choice that outlives one change | `decision` (ADR) | `docs/decisions/` |
| Real work you deliberately defer | `backlog` | `docs/backlogs/` |
| The end of a work session worth recording | `worklog` | `docs/worklogs/` |
| The end of a milestone | `retro` | `docs/worklogs/` |

> No `task` artifact: the plan's **Implementation steps** checklist is the work breakdown (link Jira from there if tracked externally).

## Working protocol

1. **Intake first.** Classify the input + pick a lane `tiny`/`normal`/`high-risk` (`docs/framework/06-intake.md` + `docs/framework/05-risk.md`), then read out the Output block. The lane decides which artifact is needed first, who approves, and how far proof must go. Hard gates (auth, data loss, audit, external vendor, removing a verification) → high-risk, stop and ask the human before acting.
2. **Read before writing.** Scan `docs/wiki/<topic>.md` (the current state of the capability), plus the relevant `docs/decisions/` and `docs/plans/` before starting.
3. **Plan before implementing.** For lane `normal`+, write the plan (technical design, local decisions, implementation steps) and get it `approved` by the human before coding — reviewing a plan is cheaper than reviewing a diff. Tick the plan's Implementation steps as you go.
4. **Propose the missing artifact.** If a decision that outlives the change is being made in conversation with no ADR, say so and offer to record it.
5. **Tie proof to "done".** For lane `normal`+, fill the Proof table in the story/plan — not done until there is verification evidence matching the lane.
6. **Update the wiki on ship.** A change that touches a capability → overwrite `docs/wiki/<topic>.md` to match the as-built (mandatory, see phase 6 in `docs/framework/02-lifecycle.md`). If the topic has no page yet, create one from `docs/templates/wiki.md`.
7. **Write a worklog at session end.** When you finish a meaningful chunk of work, create a `worklog` with frontmatter (`lane`, `outcome`, `touched`, `files_changed`, `friction`) + a summary: what you did, why, what is still open.
8. **Decision boundary.** Agents propose; humans approve the irreversible decisions (see `docs/framework/01-roles.md`).

## File naming

Per `docs/framework/04-conventions.md`. Summary: `<NNNN>-<slug>.md`, numbers increasing within each `docs/<type>/` directory.
