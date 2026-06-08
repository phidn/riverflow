# 06 — Intake

Every request that leads to a code or artifact change passes through the **intake gate** before
you edit. Intake **creates no new file** — it is a classification step the agent runs itself,
ending in an **Output block** read aloud in the conversation. The human does not have to
classify risk; the agent does.

Intake is the **input** counterpart to the worklog (the output). [00-overview](00-overview.md)
says "record at the point of decision"; intake adds "classify at the point of intake" — so a
request never slides straight into code without anyone knowing what kind of work it is, which
docs it touches, or how much proof it needs.

## Why this gate

The old gate — "before coding, check whether a story exists" — was an **invisible** marker:
in a conversation, "before coding" is not a moment, it is a blur, so it never fired. The ADR
(recorded at the point of decision) and the worklog (recorded at session end) survive because
their triggers are **intrinsic** to the work. Intake gives the intent layer an intrinsic trigger
too: **it runs on every request**, and it **forces an Output block to be read out**. A miss
becomes visible on every turn, instead of quietly slipping past once.

## The intake flow

```
Request
  → Classify the input (Input Type)
  → Restate it as a work item
  → Read the relevant topic wiki (docs/wiki/<topic>.md) + story / ADR
  → Run the risk checklist (05-risk)
  → Pick a lane
  → Read out the Output block
```

## Axis 1 — Input Type

The lane ([05-risk](05-risk.md)) answers "how risky". The input type answers a different
question: which artifact does this **land on**. Two independent axes — choose both.

| Type | When | Typical artifact |
| --- | --- | --- |
| New brief | a new large product/feature goal | `product-brief` → story/ADR |
| Story slice | implementing part of already-agreed behavior | `user-story` |
| Change request | fix / change / tune existing behavior | `story` **or** a direct patch |
| New initiative | a large area needing many stories | a thin `spec` (index) + many stories |
| Maintenance | technical / dependency / operational change | `story` / `worklog` / `ADR` |
| Framework improvement | changes how human↔agent collaborate | update the framework + `worklog` |

> Don't build one giant monolithic spec. The living surface is story + ADR + (when needed) a
> **thin spec** as an index. A thin spec only answers "what does the feature do end-to-end +
> what is done + the proof table", with most of the content gathered from existing ADRs/worklogs.

## Catching emergence

A feature that emerges gradually is **invisible at the first request**: "paste a dub URL and
let's try it" looks exactly like a small change-request. Intake does **not** try to predict it.
The mechanism is **repeated visibility**: intake runs again on every request, and the Output
block forces the `Story` line to be restated. The hard rule:

> When the **second** request touches the same topic and there is still no story/spec → stop,
> propose an initiative (a thin spec + story). When a chunk of work spans **≥2 sessions** → it is
> real, it needs a home.

One question instead of the whole old gate:

> **"Can I answer *is this done yet* by myself, without reading the code?"**
> Answer "no" → you need a story/spec, no matter which session you're in.

When real-but-not-yet work appears (a good idea mid-session, a "still open" you won't do now, a
request outside the current scope) → write a **backlog item** (`docs/backlogs/`, see
[03-artifacts](03-artifacts.md)) instead of letting it drift or cramming it into the work at
hand. The backlog is intake's relief valve: it keeps the work without steering the current
session off course.

## The Output block (forcing function)

Before editing code/artifacts, the agent reads out this block:

```
Lane:   normal
Type:   change request
Docs:   spec-0002, ADR-0004
Wiki:   docs/wiki/rbac.md (read first; update on ship)
Story:  docs/stories/0003-... (or: none yet → propose creating | not needed — tiny patch)
Proof:  unit + integration
```

The `Story` line forces a **definite** choice: point to an existing file name, **propose
creating one**, or **say outright "not needed — tiny patch"** with a reason. There is no fourth
option that is silence. This is the entire anti-miss mechanism.

The `Wiki` line closes both ends: the topic page name to **read first** (recall), and a reminder
that the page must be **updated on ship** (see phase 6, [02-lifecycle](02-lifecycle.md)). A topic
with no page yet → write "none yet → create on ship"; a change that touches no capability worth
recording → "not needed".

## Lane → what intake needs (ties to 05-risk)

| Lane | Output | Human approves first? |
| --- | --- | --- |
| tiny | a concise Output block; patch directly | no |
| normal | + a story named / proposed + a proof table | only if hard to reverse |
| high-risk | + an ADR if there is a big choice + human confirmation before acting | yes |

## Closing the loop with the worklog

The end-of-session worklog ([04-conventions](04-conventions.md)) adds an `input_type` field to
its frontmatter, so you can later grep "what type did this work come **in** as → what result did
it go **out** as". Intake is the ephemeral input (read in conversation); the worklog is where it
settles durably.
