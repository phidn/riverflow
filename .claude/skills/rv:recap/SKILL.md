---
name: rv:recap
description: Lock in and save the decisions + ways-of-working worth keeping from the current conversation into riverflow artifacts. By default it crystallizes the full package — a thin plan + stories (when the session built a feature that has none yet), plus ADR + worklog + wiki. This is the intent-layer fix for emergent work: you don't write the plan first, rv:recap crystallizes it at the end from the work you actually did. Canonical trigger is the prefix "rv:recap". Auto-selects a recommendation and writes it immediately (without asking first); the user adjusts afterward if needed. Supports targeted recap from the prefix — "rv:recap plan" / "rv:recap story" / "rv:recap wiki" / "rv:recap worklog" / "rv:recap adr" do just that part ("rv:recap spec" is a deprecated alias for "rv:recap plan"); a bare "rv:recap" does the full package. Use when the user types "rv:recap" (optionally followed by a part) or otherwise asks to save/record from this conversation into riverflow (e.g. "save to riverflow", "capture this into riverflow", "/rv:recap"). Scans the conversation, selects the candidates worth keeping, writes the files per the riverflow conventions, updates the Decision Log + the wiki.
---

# rv:recap

Turns the **worth-keeping** decisions, *intent*, and ways-of-working in the current session into
durable riverflow artifacts — **auto-selects a recommendation and writes it immediately** (without
asking first), but still **selectively**, not blindly. If the user wants it different, they'll say
so and you adjust afterward.

Source framework: the riverflow repo (see `docs/framework/`, templates in `docs/framework/templates/`).
riverflow principle: *every important decision leaves a trace readable by both humans and agents.*
This skill is the **trigger mechanism** for that principle.

## Why this also crystallizes the plan/story (the intent layer)

Real work is often **emergent**: the user prompts a problem, the agent does it, asks "ok?", the
user says "ok" — and a feature grows that way, never starting from a written plan. Plan-first
fights that flow, so the intent layer (`plan` + `story`) gets skipped and you can no longer answer
*"is this done? what's proven?"* without re-reading the code. The fix is **not** to force a plan up
front — it is to let the plan **crystallize at recap time** from the work that actually happened.
So a bare `rv:recap` now also writes a thin plan + stories **when the session built a feature that
has no plan/story yet** (see the emergence test in step 2b). For a one-off tiny patch, it does not.

## When to use

Canonical trigger: the prefix **`rv:recap`** (optionally followed by a part). Also fires on
natural phrasing — "save to riverflow", "capture this into riverflow", "/rv:recap". Do NOT
run automatically at the end of every session — only run when triggered.

## What to recap (route from the prefix)

The skill is selective by **what the user asked for**. Read the part after `rv:recap` and do only that:

| Trigger | Do |
| --- | --- |
| `rv:recap plan` (alias: `rv:recap spec`, deprecated) | only crystallize the thin `plan` (+ its stories) for the feature this session built |
| `rv:recap story` | only write/update the `user-story` (or stories) |
| `rv:recap wiki` | only update/create the related `docs/wiki/<topic>.md` |
| `rv:recap worklog` | only write the worklog |
| `rv:recap adr` / `rv:recap decision` | only write the ADR(s) |
| `rv:recap` (no part named) / "save to riverflow" | **full package**: plan + stories (only if the session built a feature with none yet — step 2b) + ADR + worklog + wiki (wiki only if a capability was shipped this session) |

Each part is independent — keep the skill small, do exactly the asked-for slice. When the user
names `plan` or `story` explicitly, write it even for small work (they asked for it); the emergence
test only gates the **automatic** plan/story in a bare `rv:recap`.

## Artifact types

Intent layer — the "what / when is it done", crystallized from the session (see step 2b):

- **Plan** — the thin plan of the *feature/change* this session built: summary, main flow,
  the key edge cases, the technical design as built, decisions made in place, acceptance criteria
  + a Proof table. Grained to the increment; frozen after ship (`done`). Acts as the **index**
  over the ADRs/stories the session produced. When recapping, the Implementation steps section
  records what was actually done (ticked), so the plan honestly reflects the work.
- **User Story** — the user-facing need(s) behind it: *"As a `<role>`, I want `<X>`, so that
  `<Y>`"* + **verifiable acceptance criteria** + a Proof table. One per finishable chunk.

Decision / output layer:

- **Decision (ADR)** — a choice that **outlives this change** (a later plan would need to look it
  up), with its reasoning + trade-offs. Choices local to the change go in the plan's **Decisions**
  section instead — don't mint an ADR for them.
- **Worklog** — what this session did, small in-place decisions, what is still open (handoff).
- **Wiki** — the living as-built of a capability the session shipped/changed, by topic (phase 6 of
  `AGENTS.md` makes this mandatory on ship).

The remaining riverflow types (brief/retro) are NOT part of this skill.

## Procedure

### 1. Detect the repo layout
riverflow has two install shapes; auto-detect the target directory:

- **Project repo (standard riverflow):** `docs/decisions/`, `docs/worklogs/`, `docs/plans/`,
  `docs/stories/`, templates in `docs/framework/templates/`.
- **Embedded install:** `decisions/`, `worklogs/`, `plans/`, `stories/` at the root, a
  `decisions.md` router, templates in `framework/templates/`.

Detect in order: if there is a `decisions.md` at the root → embedded layout (`decisions/` +
`worklogs/` + `wiki/` + `plans/` + `stories/`). If there is a `docs/decisions/` → standard
riverflow (plans at `docs/plans/`, stories at `docs/stories/`, wiki at `docs/wiki/`). An older
install may still have `specs/` instead of `plans/` — treat it as the plans directory and note the
rename to the user. If neither directory exists → ask the user whether they want to initialize a
decisions layer before continuing. Find the `plan.md` / `user-story.md` / `decision.md` /
`worklog.md` / `wiki.md` templates in the repo's templates directory (an older install may have
`spec.md` — use it the same way); if missing, take them from the riverflow repo's
`docs/framework/templates/`.

### 2. Scan the conversation → list candidates
Re-read the current session and extract:
- **The feature/intent**: what need did this session actually serve, end-to-end? → plan/story
  candidate (gated by step 2b).
- **Big decisions** (chose X over Y, changed a convention, locked in a direction) → ADR candidates
  if they outlive the change; otherwise they land in the plan's Decisions section.
- **Completed chunks worth a handoff** → worklog candidates.
- **Verification evidence produced** (tests added/run, commands, capture/QA reports, screenshots)
  → this seeds the **Proof table** of the plan/story; note the actual links/commands.

Skip trivia that only matters to the current conversation (per `docs/framework/03-artifacts.md`:
only save what will be needed next session/next week).

### 2b. Emergence test — does this session need a plan/story?
For a **bare** `rv:recap` (full package), decide whether to auto-crystallize the intent layer.
Create a thin plan + story **only if the session built a real feature/capability**, judged by
riverflow's own emergence rule (`docs/framework/06-intake.md`):

- **"Can I answer *is this done yet* without reading the code?"** — if **no** → it needs a
  plan/story.
- Other yes-signals: the work spanned **≥2 sessions** or **≥2 requests on the same topic**; it
  shipped a user-facing capability; it changed a convention broadly.

If the work is a **one-off tiny patch** (could be a single `tiny`-lane Output block, "is it done"
is obvious) → **skip** plan/story; just do ADR/worklog/wiki as before, and say one line why.

Before creating, **check for an existing plan/story** on this topic (scan `plans/` + `stories/`
titles). If one exists → **update it** (flip status, fill the Proof table, add acceptance items)
rather than creating a duplicate. When the user explicitly typed `rv:recap plan`/`story`, skip this
gate — they asked for it; still prefer updating an existing file over duplicating.

### 3. Auto-select a recommendation and apply it (without asking first)
Do NOT stop to ask the user to choose. Decide yourself which candidates are worth keeping (by the
step 2 criteria — only what will be needed next session/next week) and **write them** in step 5.
When the user wants it different, they'll say so — adjust then.

Still list the selected candidates briefly (type + title) for transparency, but treat that as a
"saving now" notice, not a blocking question. For example:

```
Saving from this session (recommended):
  [1] Plan    — Live streaming pipeline (thin index) · Proof: seeded from e2e run
  [2] Story   — Stream stays sharp on slow uplinks · acceptance met
  [3] ADR     — Stream output at 1080p30
  [4] Worklog — Fix YouTube Live going blurry at 720p
```

(Omit the Plan/Story lines for a one-off tiny patch — step 2b.)

If there is a borderline candidate (questionable whether to keep), skip it and say one line why —
don't save blindly.

### 4. Pick the risk lane
For each ADR (and for the worklog), pick the lane per `docs/framework/05-risk.md`
(`tiny`/`normal`/`high-risk`). Hard gate → high-risk, confirm with the human first.

### 5. Write the files
Write the **intent layer first** (plan → stories), then ADR → worklog → wiki, so the later
artifacts can link back to the plan/story by id.

- **Plan:** only if step 2b says so. Next sequence number = (max NNNN in the plans directory) + 1,
  four digits. Name `<NNNN>-<slug>.md`, fill the `plan.md` template. Keep it **thin** — it is an
  index over the session's stories/ADRs, not a re-plan. `Status`: `done` if the feature was built
  **and** verified this session, else `approved`/`draft`. Fill `Related stories` + `Related
  ADRs` with the ids you create below. Put the in-place choices in the **Decisions** section, the
  work actually done (ticked) in **Implementation steps**, and fill the **Proof** table from the
  evidence found in step 2 (real links/commands); mark tiers honestly (`not yet` where there is no
  evidence).
- **Story:** one per finishable chunk the session delivered (often 1–3). Next number from the
  stories directory. Name `<NNNN>-<slug>.md`, fill `user-story.md`: the `As a <role>…` line,
  **verifiable acceptance criteria** (tick the ones actually met this session), `Lane`, `Related
  plan` → the plan above, and the **Proof** table seeded from real evidence. `Status`: `done` only
  if acceptance is met with proof, else `in progress`.
- **ADR:** only for choices that outlive the change. Next sequence number = (max NNNN currently in
  the decisions directory) + 1, four digits. Name `<NNNN>-<slug>.md`. Fill the `decision.md`
  template. Status defaults to `proposed`; set `accepted` only if the user clearly locked it in
  during the conversation. Date = today (ask if unsure — don't invent). Decided by = the user if
  it is a hard-to-reverse decision.
- **Worklog:** name `YYYY-MM-DD-<slug>.md`. Fill the frontmatter (`date/who/lane/outcome/
  touched/files_changed/friction`) + the body per the `worklog.md` template. Set `touched` to the
  plan/story/ADR ids created in this recap so the session links to its intent layer.
- **Wiki:** name `<topic-slug>.md` in the wiki dir (one topic = one page, overwritten in place).
  **Infer the topic from the session and just write it** — we can fix it later, and this is mostly
  read by agents, so bias to action over asking. Only ask a one-line "which topic?" when it is
  genuinely ambiguous (the session touched several capabilities and the prompt didn't name one).
  Fill from the `wiki.md` template; set `updated` / `verified_against_code` to today, and
  `source_plans` to the plan id if one was written this recap. If a page for the topic already
  exists, **overwrite to match the as-built**, don't append.
- `slug` kebab-case, English (or another language with correct orthography — never a lossy unaccented form).

### 6. Update the log + confirm
- If the repo has a `decisions.md` router with a **Decision Log** table: add a row for each new ADR.
- Summarize for the user: which files were created/overwritten, where (incl. any wiki page touched).
- Ask whether to `gitx push` (don't push on your own). Use `gitx`, not bare `git`, for GitHub operations.

## Boundaries

- The skill auto-selects a recommendation and writes it immediately, without asking first; the user
  adjusts afterward if needed. Still be selective — only save what is worth keeping, not session trivia.
- When the prompt names a part (`plan` / `story` / `wiki` / `worklog` / `adr`), do **only** that
  slice — don't recap the rest. A bare `recap` does the full package (plan/story-if-emergent + ADR
  + worklog + wiki-if-shipped).
- Plan/story is **auto-created only when the session built a real feature** (step 2b); a one-off
  tiny patch gets no plan/story. Don't write the plan first or plan ahead — crystallize from what
  was actually done. Always prefer **updating** an existing plan/story over creating a duplicate.
- Wiki is overwritten in place to match the as-built — never append a changelog to it. Bias to
  inferring the topic and writing; ask only when genuinely ambiguous.
- Don't delete/edit an `accepted` ADR. Changed your mind → a new ADR, mark the old one `superseded by`.
- Keep artifacts short (ADR half a page, worklog a few items). Don't copy the whole conversation into the file.
