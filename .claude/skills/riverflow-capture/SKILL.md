---
name: riverflow-capture
description: Lock in and save the decisions + ways-of-working worth keeping from the current conversation into riverflow artifacts (ADR + worklog + wiki). Canonical trigger is the prefix "rv:recap". Auto-selects a recommendation and writes it immediately (without asking first); the user adjusts afterward if needed. Supports targeted recap from the prefix — "rv:recap wiki" / "rv:recap worklog" / "rv:recap adr" do just that part; a bare "rv:recap" does the full package. Use when the user types "rv:recap" (optionally followed by wiki/worklog/adr) or otherwise asks to save/record from this conversation into riverflow (e.g. "save to riverflow", "capture this into riverflow", "/riverflow-capture"). Scans the conversation, selects the candidates worth keeping, writes the files to riverflow spec, updates the Decision Log + the wiki.
---

# riverflow-capture

Turns the **worth-keeping** decisions and ways-of-working in the current session into durable
riverflow artifacts — **auto-selects a recommendation and writes it immediately** (without asking
first), but still **selectively**, not blindly. If the user wants it different, they'll say so and
you adjust afterward.

Source framework: the riverflow repo (see `docs/framework/` and `docs/templates/`).
riverflow principle: *every important decision leaves a trace readable by both humans and agents.*
This skill is the **trigger mechanism** for that principle.

## When to use

Canonical trigger: the prefix **`rv:recap`** (optionally followed by a part). Also fires on
natural phrasing — "save to riverflow", "capture this into riverflow", "/riverflow-capture". Do NOT
run automatically at the end of every session — only run when triggered.

## What to recap (route from the prefix)

The skill is selective by **what the user asked for**. Read the part after `rv:recap` and do only that:

| Trigger | Do |
| --- | --- |
| `rv:recap wiki` | only update/create the related `docs/wiki/<topic>.md` |
| `rv:recap worklog` | only write the worklog |
| `rv:recap adr` / `rv:recap decision` | only write the ADR(s) |
| `rv:recap` (no part named) / "save to riverflow" | **full package**: ADR + worklog + wiki (wiki only if a capability was shipped this session) |

Each part is independent — keep the skill small, do exactly the asked-for slice.

## Three artifact types

- **Decision (ADR)** — an important, hard-to-reverse choice, with its reasoning + trade-offs.
- **Worklog** — what this session did, small in-place decisions, what is still open (handoff).
- **Wiki** — the living as-built of a capability the session shipped/changed, by topic (phase 6 of
  `AGENTS.md` makes this mandatory on ship).

The other riverflow types (brief/story/spec/task/retro) are NOT part of this skill.

## Procedure

### 1. Detect the repo layout
riverflow has two install shapes; auto-detect the target directory:

- **Project repo (standard riverflow):** `docs/decisions/`, `docs/worklogs/`, templates in `docs/templates/`.
- **Embedded install:** `decisions/`, `worklogs/` at the root, a `decisions.md` router, templates in `templates/`.

Detect in order: if there is a `decisions.md` at the root → embedded layout (`decisions/` +
`worklogs/` + `wiki/`). If there is a `docs/decisions/` → standard riverflow (wiki at
`docs/wiki/`). If neither directory exists → ask the user whether they want to initialize a
decisions layer before continuing. Find the `decision.md` / `worklog.md` / `wiki.md` templates in
the repo's templates directory; if missing, take them from the riverflow repo's `docs/templates/`.

### 2. Scan the conversation → list candidates
Re-read the current session and extract:
- **Big decisions** (chose X over Y, changed a convention, locked in a direction) → ADR candidates.
- **Completed chunks worth a handoff** → worklog candidates.

Skip trivia that only matters to the current conversation (per `docs/framework/03-artifacts.md`:
only save what will be needed next session/next week).

### 3. Auto-select a recommendation and apply it (without asking first)
Do NOT stop to ask the user to choose. Decide yourself which candidates are worth keeping (by the
step 2 criteria — only what will be needed next session/next week) and **write them** in step 5.
When the user wants it different, they'll say so — adjust then.

Still list the selected candidates briefly (type + title) for transparency, but treat that as a
"saving now" notice, not a blocking question. For example:

```
Saving from this session (recommended):
  [1] ADR  — Stream output at 1080p30
  [2] Worklog — Fix YouTube Live going blurry at 720p
```

If there is a borderline candidate (questionable whether to keep), skip it and say one line why —
don't save blindly.

### 4. Pick the risk lane
For each ADR (and for the worklog), pick the lane per `docs/framework/05-risk.md`
(`tiny`/`normal`/`high-risk`). Hard gate → high-risk, confirm with the human first.

### 5. Write the files
For each selected candidate:
- **ADR:** next sequence number = (max NNNN currently in the decisions directory) + 1, four
  digits. Name `<NNNN>-<slug>.md`. Fill the `decision.md` template. Status defaults to
  `proposed`; set `accepted` only if the user clearly locked it in during the conversation. Date =
  today (ask if unsure — don't invent). Decided by = the user if it is a hard-to-reverse decision.
- **Worklog:** name `YYYY-MM-DD-<slug>.md`. Fill the frontmatter (`date/who/lane/outcome/
  touched/files_changed/friction`) + the body per the `worklog.md` template.
- **Wiki:** name `<topic-slug>.md` in the wiki dir (one topic = one page, overwritten in place).
  **Infer the topic from the session and just write it** — we can fix it later, and this is mostly
  read by agents, so bias to action over asking. Only ask a one-line "which topic?" when it is
  genuinely ambiguous (the session touched several capabilities and the prompt didn't name one).
  Fill from the `wiki.md` template; set `updated` / `verified_against_code` to today. If a page for
  the topic already exists, **overwrite to match the as-built**, don't append.
- `slug` kebab-case, English (or another language with correct orthography — never a lossy unaccented form).

### 6. Update the log + confirm
- If the repo has a `decisions.md` router with a **Decision Log** table: add a row for each new ADR.
- Summarize for the user: which files were created/overwritten, where (incl. any wiki page touched).
- Ask whether to `gitx push` (don't push on your own). Use `gitx`, not bare `git`, for GitHub operations.

## Boundaries

- The skill auto-selects a recommendation and writes it immediately, without asking first; the user
  adjusts afterward if needed. Still be selective — only save what is worth keeping, not session trivia.
- When the prompt names a part (`wiki` / `worklog` / `adr`), do **only** that slice — don't recap
  the rest. A bare `recap` does the full package (ADR + worklog + wiki-if-shipped).
- Wiki is overwritten in place to match the as-built — never append a changelog to it. Bias to
  inferring the topic and writing; ask only when genuinely ambiguous.
- Don't delete/edit an `accepted` ADR. Changed your mind → a new ADR, mark the old one `superseded by`.
- Keep artifacts short (ADR half a page, worklog a few items). Don't copy the whole conversation into the file.
