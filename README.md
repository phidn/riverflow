# riverflow

**Collaborative flow** — a framework that defines how humans and agents work together across the whole software development lifecycle.

riverflow is not code. It is a set of **conventions + document templates** that answer one question: when a human and an agent build a product together, *what information must be written down, where, in what format, and who owns which part.*

## Why riverflow exists

When an agent joins the development loop, most of the context lives in a human's head or inside a single chat session and then disappears. riverflow turns decisions, requirements, and rationale into **durable artifacts** that both the human and any later agent session can read — instead of reconstructing them from scratch every time.

Root principle: **every important decision and every important requirement must leave a trace readable by both humans and agents.**

## Inspiration — *Under the River*

riverflow takes its name and its core bet from Shopify's essay [*Under the River*](https://shopify.engineering/under-the-river).

In that essay, **River** is the visible agent and **Aquifer** is the durable substrate beneath it — the session layer, sandbox isolation, and append-only event logs that persist when processes die. Shopify's key observation: *each agent session leaves artifacts behind, so the next person starts from the previous thread's knowledge, not a blank prompt. The codebase teaches the agent; the agent teaches the codebase; both teach the humans watching.*

riverflow is the **lightweight, single-operator counterpart** of that idea. Shopify needed a Postgres-backed substrate to make agent knowledge compound across a whole company. riverflow makes the same knowledge compound for one person and their agents — using **nothing but Markdown, no infrastructure.** The river is the ephemeral session that flows past; the artifacts are the riverbed that stays and shapes where the next flow runs.

## Repo structure

```
riverflow/
├── README.md              # this file
├── AGENTS.md              # rules for agents working in a repo that follows riverflow
├── .claude/skills/        # Claude Code skill shipped with riverflow
│   └── riverflow-capture/ # lock in & save decisions from a conversation → artifacts
└── docs/
    ├── framework/             # framework definition (the meta layer)
    │   ├── 00-overview.md     # philosophy + the human ↔ agent collaboration model
    │   ├── 01-roles.md        # roles & responsibility boundaries, human / agent
    │   ├── 02-lifecycle.md    # phases: discovery → spec → decision → build → review → ship
    │   ├── 03-artifacts.md    # catalog of document types & when to create which
    │   ├── 04-conventions.md  # naming, storage location, handoff protocol, worklog frontmatter
    │   ├── 05-risk.md         # risk lanes (tiny/normal/high-risk) + hard gates
    │   └── 06-intake.md       # input classification gate + the Output block before editing
    ├── templates/             # template for each document type
    │   ├── decision.md        # ADR — Decision Record
    │   ├── user-story.md      # story + acceptance criteria
    │   ├── product-brief.md   # problem, goals, non-goals, metrics
    │   ├── spec.md            # PRD / spec for one change
    │   ├── wiki.md            # living as-built record of a capability (by topic)
    │   ├── task.md            # work item (links to Jira)
    │   ├── backlog.md         # deliberately deferred work — picked up later as story/spec/task
    │   ├── worklog.md         # an agent's session log
    │   └── retro.md           # review / lessons learned
    └── (real instances — living examples)
        ├── decisions/
        ├── stories/
        ├── specs/
        ├── wiki/
        ├── backlogs/
        └── worklogs/
```

## Install — let Claude do it

The simplest way to add riverflow to a project: paste this prompt to Claude (in Claude Code, from your project root).

```
Clone https://github.com/phidn/riverflow into a temp dir, then set my project up to
follow riverflow: copy its `docs/framework/` and `docs/templates/` into my repo, copy
its `.claude/skills/riverflow-capture/` skill into my `.claude/skills/`, add an
`AGENTS.md` based on riverflow's, and create the empty `docs/{decisions,stories,
specs,wiki,backlogs,worklogs}/` folders. Read riverflow's README + AGENTS.md first,
then summarize the conventions back to me.
```

Claude reads the framework, copies the conventions, templates, and the `riverflow-capture` skill into your repo, and scaffolds the `docs/` layout — no manual setup.

## How to use it

1. Read `docs/framework/00-overview.md` to get the model.
2. When you start a new product/feature, copy the matching template from `docs/templates/` into `docs/<type>/`.
3. Name files per `docs/framework/04-conventions.md`.
4. Agents read `AGENTS.md` before creating or editing any artifact.

**Maturity stance:** riverflow deliberately stays a **lightweight, pure-Markdown method** — it borrows selectively from mature ideas (risk lanes, schema'd worklogs, proof tables) at "Markdown weight," and adds **no** tooling/DB. This is a deliberate stopping point: enough discipline to use for real, still read-it-and-understand-it, and applicable even to non-code products. Heavy automation (verification runners, self-scoring observability, self-improving loops) belongs to an engine with tooling — not to riverflow.
