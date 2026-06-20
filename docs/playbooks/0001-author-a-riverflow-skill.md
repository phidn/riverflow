---
name: author-a-riverflow-skill
when_to_use: You want to add a new skill to a riverflow-style framework repo and want intent logged before code.
source: riverflow / 2026-06-20 session (the rv:playbook skill itself)
inputs: [a skill idea or felt friction, a riverflow repo with docs/ + .claude/skills/]
produces: [plan, backlog, playbook, worklog]
maturity: draft
---

# Playbook: Author a new riverflow skill

> A **portable** how-to: the loop for taking a "we should have a skill for X" idea to a shipped,
> standalone skill — with the intent reviewed *before* any code. Distilled from building the
> `rv:playbook` skill; reusable for any new `rv:*` (or project) skill.

## Goal

Turn a skill idea into a working, standalone `SKILL.md` (+ any template/artifact it produces),
with a reviewed `draft`→`approved` plan in front of the human before implementation, and a backlog
trail for anything deliberately deferred.

## When to use / preconditions

- Trigger: a recurring need that a skill would automate, or "let's brainstorm a skill for `<X>`".
- Preconditions: a riverflow repo with `docs/` (plans, decisions, backlogs, templates) and a
  `.claude/skills/` directory; existing skills to use as style references.

## Inputs needed

| Input | What it is | Where it comes from |
| --- | --- | --- |
| `<skill idea>` | the friction or capability the skill should cover | the human / a felt pain in a long session |
| `<reference skills>` | 1–2 existing skills to match tone & structure | `.claude/skills/` (e.g. `rv:recap`, `rv:brainstorm`) |
| `<templates>` | the plan/backlog/target-artifact templates | `docs/framework/templates/` |

## Steps

### 1. Enter via brainstorm — recall + intake

- **Purpose:** anchor the idea in what the repo already knows before generating options.
- **Inputs:** `<skill idea>`, the existing `docs/` tree.
- **Do:** run `rv:brainstorm`; detect layout; read existing skills, plans, decisions, backlogs on
  the topic; summarize the starting state in 3–5 lines; run the intake gate (lane + Output block).
- **Output:** a recall summary + intake readout → no file yet.
- **Decision points:** lane `tiny` → skip the plan, just patch; hard gate → flag high-risk.

### 2. Surface the core design tension, then ask focused decision questions

- **Purpose:** the questions that actually shape the skill are the ones to put to the human — not a
  wall of trivia.
- **Inputs:** the recall.
- **Do:** name the one or two tensions that fork the whole design (e.g. *form of the output*,
  *where it lives*, *scope: standalone vs core*), present 2–3 genuinely different options each with
  trade-offs, recommend a front-runner.
- **Output:** the human's chosen direction (their call).
- **Decision points:** if a choice outlives this change → flag it for an ADR; off-scope good ideas
  → backlog.

### 3. Converge → write the draft plan (+ defer the rest to backlog)

- **Purpose:** make the intent reviewable before any code.
- **Inputs:** the chosen direction.
- **Do:** write `docs/plans/<NNNN>-<slug>.md` from `docs/framework/templates/plan.md`, **Status: draft** —
  fill Technical design (the files to add), Decisions, Implementation steps, Proof tiers. Create a
  `docs/backlogs/<NNNN>-*.md` for anything deliberately deferred (e.g. graduating standalone→core).
- **Output:** a `draft` plan + backlog item.
- **Decision points:** a decision that constrains future work → ADR now; local-only → plan
  Decisions section.

### 4. Hand off for review — stop until approved

- **Purpose:** reviewing a plan is cheaper than reviewing a diff; approval is the human's move.
- **Do:** summarize plan path + gist + lane; state the gate explicitly; do **not** start coding.
- **Output:** none until the human says "approve".
- **Decision points:** human approves → flip Status to `approved`; redirects → revise the plan.

### 5. Implement the skill

- **Purpose:** build what the approved plan specifies.
- **Inputs:** the `approved` plan, `<reference skills>`, `<templates>`.
- **Do:** write the target artifact's template first (if the skill produces one), then
  `.claude/skills/<name>/SKILL.md` (frontmatter `name` + a rich `description` that encodes the
  trigger; body = Procedure + Boundaries). Match an existing skill's structure. Tick plan steps as
  you go.
- **Output:** `docs/framework/templates/<artifact>.md` (if any) + `.claude/skills/<name>/SKILL.md`.
- **Decision points:** standalone-first → do **not** wire framework distribution surfaces
  (README / update-version / artifact catalog); that is the graduation step.

### 6. Smoke-test + worklog

- **Purpose:** prove the skill runs and record the session.
- **Do:** run the new skill once against a real input (often this very conversation) to produce its
  first artifact; write a `docs/worklogs/` entry (lane, outcome, touched, files_changed, friction).
- **Output:** the skill's first real artifact + a worklog.
- **Decision points:** if the smoke test reveals gaps → loop back to step 5.

## Pitfalls & lessons

- **Don't over-ask in the brainstorm.** Only the design-forking questions belong in the human
  prompt; mechanical choices you can pick and mention.
- **Name carefully.** "workflow" clashed with Claude Code's Workflow-orchestration concept →
  "playbook" was clearer. Check for terminology collisions before committing to a name.
- **Standalone-first de-risks framework changes.** Keep the new skill out of README /
  `rv:update-version` / the artifact catalog until it's proven; backlog the graduation.
- **The skill `description` is the real interface** — it carries the trigger phrases and the
  when-to-use/when-not boundary. Spend effort there, not just the body.

## Parameters to fill per project

| Placeholder | Meaning | Example (from source) |
| --- | --- | --- |
| `<name>` | the skill's name/dir | `rv:playbook` |
| `<skill idea>` | the capability to automate | "distill a conversation into a reusable workflow doc" |
| `<artifact>` | the artifact the skill produces (if any) | `playbook` (`docs/framework/templates/playbook.md`) |
| `<reference skills>` | skills copied for structure | `rv:recap`, `rv:brainstorm` |
| deferred work | what goes to backlog | graduate playbook standalone → framework core |

## Related riverflow artifacts

- **plan** — step 3 (the reviewable intent, `draft`→`approved`→`done`).
- **backlog** — step 3 (deferred graduation / off-scope ideas).
- **ADR** — step 2/3, only if a choice outlives this change.
- **worklog** — step 6 (session record).
- the skill's **own output artifact** — step 5–6 (e.g. a new template + its first instance).
