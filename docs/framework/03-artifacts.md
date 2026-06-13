# 03 — Artifacts

The catalog of riverflow's standard document types. Each type exists to answer one specific future question. The matching template lives in `docs/templates/`.

## Summary table

| Type | The question it answers | Template | Stored in |
| --- | --- | --- | --- |
| Product Brief | What problem do we solve, for whom, what is success? | `product-brief.md` | `docs/plans/` |
| User Story | What does the user need, and when is it done? | `user-story.md` | `docs/stories/` |
| Plan | What does *this change* commit to, how is it built, what proof passes? | `plan.md` | `docs/plans/` |
| Wiki | How does *this capability* work right now? | `wiki.md` | `docs/wiki/` |
| Decision (ADR) | Why this direction over the others — *beyond this one change*? | `decision.md` | `docs/decisions/` |
| Backlog | What did we deliberately defer, why, when to pick it up? | `backlog.md` | `docs/backlogs/` |
| Worklog | What did the last session do, why, what is still open? | `worklog.md` | `docs/worklogs/` |
| Retro | What did the last milestone teach us? | `retro.md` | `docs/worklogs/` |

> There is no separate `task` type. A plan carries its own **Implementation steps** checklist;
> work tracked externally (Jira) is linked from there.

## Each type in detail

### Product Brief
The opening document for a large product/feature. Minimum content: **problem, target user, goals, non-goals, success metrics**. Short — one page is enough. Non-goals matter as much as goals: they block scope creep. Stored alongside plans in `docs/plans/` — a brief is intent too, frozen once approved.

### User Story
A user need stated briefly, with **verifiable acceptance criteria**. The familiar format: *"As a <role>, I want <something>, so that <benefit>."* A story should be small enough to finish within one chunk of work. The story is the **demand side** (what is needed, is it done yet — it carries status, lane, priority); the plan is the **supply side** (how this change works). Stories are **optional by lane**: `tiny` work needs none; one plan may serve several stories.

### Plan
The technical design document of one **change** — the artifact you write after brainstorming and **review before implementing**. It covers the what (summary, main flow, edge cases, constraints, acceptance criteria + proof) **and** the how (technical design, local decisions, implementation steps as a checklist). A plan is grained to *a specific increment* — anchored to the moment that change was made. Its content **freezes after ship** (it keeps a trace, like an ADR); it does *not* describe the current state of the whole topic — that is the Wiki's job. Links to the relevant stories and decisions.

Status doubles as the review gate: `draft → approved → done`. For lane `normal`+, a human approves the plan (the cheap point to correct course) before implementation starts. The **Implementation steps** checklist is ticked as work proceeds, so a later session can resume mid-plan.

The **Decisions** section holds choices local to this change. **Promotion rule:** a decision that will constrain changes beyond this one gets its own ADR instead — ask *"six months from now, working on something else, would anyone need to look this up?"* If yes → ADR; if an inline decision turns out to matter later, promote it then.

### Wiki
The **living as-built** document of a **topic/capability** (rbac, billing, auth): how that capability *currently* works, where the code is, which ADRs it rests on. It is grained by *topic* — it accumulates across many plans/sessions that individual plans cannot self-merge. **Overwritten in place**: each ship touching this capability updates the matching page. The frontmatter (`updated`, `verified_against_code`, `source_plans`) is a **freshness signal** so an agent knows whether the page is still trustworthy or has gone stale. Named by topic slug (`docs/wiki/rbac.md`), not numbered. → [ADR-0002](../decisions/0002-llm-wiki-as-built-layer.md).

**The Plan ↔ Wiki boundary** (drawn by two axes, not by metaphor):

| | Plan | Wiki |
| --- | --- | --- |
| Scope | one *change* | one *topic/capability* |
| Answers | "what does this change commit to, how is it built, what proof passes?" | "how does this capability work right now?" |
| Lifecycle | frozen after ship | living, overwritten on each related ship |

### Decision (ADR)
A decision and its reasoning, for choices that **outlive a single change** — they constrain many future plans and get looked up by topic, not by which change produced them. Structure: **context → options & trade-offs → decision → consequences**. It has a status: `proposed` → `accepted` → (possibly) `superseded`. **Never delete** an accepted ADR; if you change your mind, write a new ADR and mark the old one `superseded by`. Decisions local to one change live inline in that plan's **Decisions** section instead (see the promotion rule above) — the ADR bar is *"matters beyond this change"*, not *"matters at all"*.

### Backlog
Work you have **decided to defer**, not drop — not now, but worth keeping so a later session doesn't have to rethink it. One file per item, `<NNNN>-<slug>.md` in `docs/backlogs/`, with frontmatter (`status`, `lane_est`, `source`, `promoted_to`). Backlog is the **home of "what is still open"**: the `Still open` items of a worklog and the surfaced-but-not-yet-up requests ([06-intake](06-intake.md)) land here instead of drifting away. When picked up, an item **graduates** into a story/plan: change `status: promoted`, fill `promoted_to`, **do not delete** (keep the trace, like an ADR). An item with no clear pick-up trigger should be `dropped` rather than kept forever — the backlog is not a junk drawer.

### Worklog
The log of one work session. This is the main handoff mechanism between agent sessions and between agent ↔ human: **what was done, what was decided in place, what is still open, the next step.** Cheap to write, priceless when a later session opens the repo. Each worklog opens with a frontmatter block (`lane`, `outcome`, `touched`, `files_changed`, `friction`) — that part is for machines/agents to grep fast; the body is for humans. Detail scales with lane: `tiny` needs only `outcome` + a summary, `high-risk` fills it all.

### Retro
A milestone wrap-up: what worked, what didn't, what to change next time. Feeds the discovery phase of the next cycle.

## Risk lane & proof

Two concepts that cut across the artifacts, light enough not to need a new document type:

- **Lane** (`tiny` / `normal` / `high-risk`) — the risk level of a piece of work; it decides
  which artifact is needed first, who approves, and how far proof goes. Recorded in the **Lane**
  field of a story and in a worklog's frontmatter. How to pick a lane: [04 → 05-risk.md](05-risk.md).
- **Proof** — a table tying "done" to verification evidence (unit / integration / e2e /
  platform), living inside the story and the plan. riverflow does not split it into a separate
  file; proof is a section inside an artifact you already have.

## How to choose an artifact

- If the information will be needed **next session/next week** → create an artifact.
- If it only matters to the **current conversation** → don't create one.
- If it is real work but **not up yet** → a backlog item; don't let it sit only in a worklog's "still open" and get forgotten.
- If you are **making a choice that outlives this change** → an ADR, right then. A choice local to the change → the plan's **Decisions** section.
- When in doubt between story and plan: story = *what is needed (is it done yet)*, plan = *how this change works*.
- When in doubt between plan and wiki: plan = *one change (frozen after ship)*, wiki = *the current state of a topic (living)*.
- Before starting: run intake (06-intake.md) — classify the input + pick a lane (05-risk.md). The higher the lane, the more artifact + proof is needed up front.
