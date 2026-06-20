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
├── .claude/skills/        # Claude Code skills — each SKILL.md frontmatter has `install:` (the single source of truth for what ships; see ADR-0006)
│   ├── rv:brainstorm/     # /rv:brainstorm — enter the lifecycle: brainstorm a problem → draft plan       (install: true)
│   ├── rv:recap/          # /rv:recap — lock in & save decisions/wiki from a conversation → artifacts      (install: true)
│   ├── rv:playbook/       # /rv:playbook — distill a conversation's reusable process → portable playbook   (install: true)
│   ├── rv:update-version/ # /rv:update-version — check installed version vs. latest on GitHub → update      (install: true)
│   └── rv:release/        # /rv:release — bump the core version + CHANGELOG                                 (install: false, CORE-ONLY)
└── docs/
    ├── framework/             # framework definition (the meta layer) — riverflow itself
    │   ├── VERSION            # current framework version (semver) — single source of truth
    │   ├── CHANGELOG.md       # what changed per version (the update check reads this)
    │   ├── 00-overview.md     # philosophy + the human ↔ agent collaboration model
    │   ├── 01-roles.md        # roles & responsibility boundaries, human / agent
    │   ├── 02-lifecycle.md    # phases: discovery → plan → decision → build → review → ship
    │   ├── 03-artifacts.md    # catalog of document types & when to create which
    │   ├── 04-conventions.md  # naming, storage location, handoff protocol, worklog frontmatter
    │   ├── 05-risk.md         # risk lanes (tiny/normal/high-risk) + hard gates
    │   ├── 06-intake.md       # input classification gate + the Output block before editing
    │   └── templates/         # blank form for each document type (copied into the dirs below)
    │       ├── decision.md        # ADR — Decision Record (choices that outlive one change)
    │       ├── user-story.md      # story + acceptance criteria
    │       ├── product-brief.md   # problem, goals, non-goals, metrics
    │       ├── plan.md            # technical design doc of one change: flows + design + decisions + steps
    │       ├── wiki.md            # living as-built record of a capability (by topic)
    │       ├── backlog.md         # deliberately deferred work — picked up later as story/plan
    │       ├── worklog.md         # an agent's session log
    │       └── retro.md           # review / lessons learned
    └── (real instances — your artifacts; everything outside framework/ is yours)
        ├── decisions/
        ├── stories/
        ├── plans/
        ├── wiki/
        ├── backlogs/
        └── worklogs/
```

## Install — let Claude do it

The simplest way to add riverflow to a project: paste this prompt to Claude (in Claude Code, from your project root).

```
Clone https://github.com/phidn/riverflow into a temp dir, then set my project up to
follow riverflow: copy its `docs/framework/` into my repo (the templates ride along
inside `docs/framework/templates/`), copy every skill under its `.claude/skills/`
whose `SKILL.md` frontmatter has `install: true` into my `.claude/skills/` (skip any
with `install: false` — those are core-only and must not be installed), add an
`AGENTS.md` based on riverflow's, and create the empty
`docs/{decisions,stories,plans,wiki,backlogs,worklogs}/` folders. Read
riverflow's README + AGENTS.md first, then summarize the conventions back to me.
```

Claude reads the framework, copies the conventions, templates (inside `docs/framework/templates/`), and every skill marked `install: true` into your repo, and scaffolds the `docs/` layout — no manual setup. The `install:` frontmatter field is the single source of truth for what ships (see ADR-0006), so adding a new skill never means editing this prompt. The framework version (`docs/framework/VERSION`) comes along inside `docs/framework/`, so your install is stamped automatically.

### Migrate from coflow

Already on **coflow** (riverflow's predecessor)? See [docs/wiki/coflow_migration.md](docs/wiki/coflow_migration.md) for the in-place migration prompt that leaves all your own artifacts untouched.

### Migrate an existing project (any docs layout)

Not on coflow, but the project **already has its own docs** — a `docs/` tree, ADRs, RFCs, design docs, a wiki, PRDs, runbooks, scattered `TODO.md`s? Use this prompt. Unlike the coflow path there is **no fixed mapping**, so it works in two passes: the agent first *surveys how your project documents things*, then *proposes a mapping* onto riverflow's artifact types and waits for your approval before moving a single file. Nothing is deleted or overwritten without your sign-off.

```
Clone https://github.com/phidn/riverflow into a temp dir and read its README +
AGENTS.md + docs/framework/ first. My project already has its own documentation
and I want to adopt riverflow without losing it. Do this in two passes:

PASS 1 — SURVEY (read-only, no changes). Inventory how this project currently
documents things: scan README(s), any docs/ tree, ADR/RFC/design-doc folders,
wikis, PRDs/specs, runbooks, CHANGELOG, and stray TODO/NOTES files. Then produce
a MAPPING TABLE — one row per existing doc (or coherent group) → the riverflow
artifact it best fits → its target path, using these heuristics:
  - problem / goals / PRD / vision        → product-brief  → docs/plans/
  - requirement / user need / acceptance  → user-story     → docs/stories/
  - design doc / RFC / change proposal     → plan           → docs/plans/
  - architecture decision / "why we chose" → decision (ADR) → docs/decisions/
  - "how X works now" / runbook / overview → wiki           → docs/wiki/
  - someday / deferred / TODO backlog      → backlog        → docs/backlogs/
  - past session notes / dev journal       → worklog        → docs/worklogs/
  Mark anything that doesn't fit as "leave in place" or "skip" with a reason.
  STOP and show me the table. Do not move anything yet.

PASS 2 — MIGRATE (only after I approve the table). Install riverflow's scaffold:
copy docs/framework/ in (templates ride inside it at docs/framework/templates/),
copy every skill under .claude/skills/ whose SKILL.md frontmatter has
`install: true` into .claude/skills/ (skip any `install: false` — core-only), add an
AGENTS.md based on riverflow's (re-point a CLAUDE.md symlink at it if present),
and create the empty docs/{decisions,stories,plans,wiki,backlogs,worklogs}/
folders. Then, per the approved table, `git mv` each source doc to its riverflow
home and renumber to the <NNNN>-<slug>.md convention; where a doc's shape differs
from the riverflow template, reword it to fit but never drop content — keep the
original prose. Do NOT delete originals I marked "leave in place". Finally,
summarize what moved where, what you reworded, and what you left untouched.
```

How it differs from the coflow path: coflow is a known predecessor so its layout maps mechanically; an arbitrary project does not, so the survey-then-confirm gate exists to keep you in the loop. The riverflow scaffold (framework, templates, skills, `AGENTS.md`) installs exactly as in a fresh install; the only judgment call is which of your existing docs becomes which artifact — and you approve that mapping before any `git mv` runs. After migrating, run `/rv:update-version` anytime to stay current.

## How to use it

1. Read `docs/framework/00-overview.md` to get the model.
2. When you start a new product/feature, type **`/rv:brainstorm <topic>`** — it walks the problem into the lifecycle (context recall → intake → options) and ends in a `draft` plan for you to approve. Or copy the matching template from `docs/framework/templates/` into `docs/<type>/` by hand.
3. Name files per `docs/framework/04-conventions.md`.
4. Agents read `AGENTS.md` before creating or editing any artifact.
5. At the end of a working session, **`/rv:recap`** crystallizes what happened back into `docs/` (plan/story if emergent, ADR, worklog, wiki). The two skills are the two halves of the loop: *brainstorm → log plan → review plan → implement → recap*.
6. To turn the *reusable process* a session followed (e.g. *elicit BRD → design → …*) into a portable, copy-to-another-project recipe, type **`/rv:playbook`** — it distills a playbook into `docs/playbooks/`. Where `rv:recap` captures *this project's intent*, `rv:playbook` captures the *portable how*.

## Versioning & updates

riverflow stamps its own version at **`docs/framework/VERSION`** (semver, single source of truth).
It lives *inside* the framework dir on purpose: it travels with `docs/framework/` on install, and
it stays namespaced away from your product's own version. `docs/framework/CHANGELOG.md` records
what changed per version.

To check whether your install is behind, type **`/rv:update-version`** (the `rv:update-version`
skill; plain phrasing like "check for riverflow updates" also works). It clones the repo into a
temp dir, compares the upstream `VERSION`
with yours, shows the changelog delta, and — only if you confirm — copies the newer
framework (templates included) and skills over, never touching your own `docs/` artifacts. See
[ADR-0003](docs/decisions/0003-self-versioning-and-update-check.md) for the design.

**Maturity stance:** riverflow deliberately stays a **lightweight, pure-Markdown method** — it borrows selectively from mature ideas (risk lanes, schema'd worklogs, proof tables) at "Markdown weight," and adds **no** tooling/DB. This is a deliberate stopping point: enough discipline to use for real, still read-it-and-understand-it, and applicable even to non-code products. Heavy automation (verification runners, self-scoring observability, self-improving loops) belongs to an engine with tooling — not to riverflow.
