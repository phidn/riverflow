# 00 — Overview

## What riverflow is

riverflow is a collaboration framework: it defines **how humans and agents jointly run a software development process**, centered on turning decisions and requirements into durable documents.

It does not replace the engineering process (CI, code review, testing). It is the **information layer** that sits on top of that process: what must be recorded, who records it, where.

## The core problem

Agents are very strong within a single session but have **no durable memory** between sessions, and humans don't have the bandwidth to re-narrate the entire context each time. The default consequences:

- Decisions get repeated or accidentally reversed because nobody remembers the old reasoning.
- A requirement lives in one person's head, and the agent guesses wrong.
- "Why are we doing it this way" vanishes the moment the chat closes.

## The solution: artifacts as shared memory

riverflow sets one rule: **every important decision and requirement leaves an artifact readable by both humans and agents.**

Artifacts are the shared source of truth. The chat is ephemeral; the file stays. A fresh agent that opens the repo and reads `docs/` can rebuild the context instead of asking from scratch.

## The three layers of riverflow

1. **Model** — human and agent are two collaborating roles, not master and servant. The human owns *intent* and *irreversible decisions*; the agent owns *execution* and *proposals*. → `01-roles.md`
2. **Lifecycle** — work flows through phases; each phase produces a characteristic artifact type. → `02-lifecycle.md`
3. **Artifacts** — the standard set of document types and a template for each. → `03-artifacts.md`, `docs/framework/templates/`

## Operating philosophy

- **Just enough documentation, no more.** Each artifact exists to answer one specific future question. Don't write to look busy.
- **Decision > description.** One line of "chose X over Y because Z" is worth more than three pages of description.
- **Record at the point of decision.** Write the ADR while you're choosing, not a week later when the reason is gone.
- **Humans approve the hard-to-reverse.** Agents propose broadly; humans approve what is expensive to undo.

## Read next

- `01-roles.md` — who does what, where the boundary is
- `02-lifecycle.md` — the phases and their artifacts
- `03-artifacts.md` — the full catalog
- `04-conventions.md` — naming & storage
- `05-risk.md` — risk-based lanes
- `06-intake.md` — the input classification gate before editing
