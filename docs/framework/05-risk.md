# 05 — Risk Lanes

riverflow sorts work into three lanes by risk level. The lane decides: which artifact is
needed before you start, who approves, and how far proof must go. This is the formalization
of the "hard to reverse → human approves" heuristic from [01-roles.md](01-roles.md) — tight
enough to be repeatable, still light because it is just a checklist.

## How to pick a lane

Count the flags that are true of the work at hand:

- [ ] Touches auth / login / sessions
- [ ] Changes a data model that already holds real data
- [ ] Touches audit / security / privacy
- [ ] Calls or changes an external system (third-party API, payments)
- [ ] Changes a public contract (API, export schema, CLI, file format)
- [ ] Affects the running behavior of current users
- [ ] Runs across multiple platforms / environments
- [ ] Verification evidence is weak or unclear
- [ ] Spans multiple domains or multiple teams

**0–1 flags** → tiny or normal. **2–3 flags** → normal (demands stronger validation).
**4+ flags** → high-risk.

## Hard gates → always high-risk

No matter how few flags you count, the following are **high-risk by default** unless they
are clearly scoped down and a human agrees:

- Auth / authorization
- Data loss, or an irreversible migration
- Audit / security
- Calling an external vendor, or touching production data
- Removing an existing verification requirement

This list matches the "hard to reverse" work that humans must approve in
[01-roles.md](01-roles.md). The risk lane is just that same boundary expressed as a process.

## The three lanes

### tiny
Docs, renames, copy, narrow fixes, low risk.
→ Edit directly, run a quick check. Minimal worklog (just `outcome` + a summary).

### normal
Story-sized behavior, bounded blast radius.
→ Create a story, link the spec/product docs, fill the proof table, do the thinnest slice.
The human approves only if it touches something hard to reverse.

### high-risk
Security, data, broad scope, public contract, multi-role/multi-platform.
→ **Human confirms before you act.** A full story + an ADR if there is a big choice +
proof to full depth. Detailed worklog (with in-place decisions and friction).

## Lane → requirements

| Lane | Artifact first | Human approves first? | Minimum proof | Worklog |
| --- | --- | --- | --- | --- |
| tiny | none | no | quick check | minimal |
| normal | story + proof table | only if hard to reverse | unit / integration | standard |
| high-risk | story + ADR + proof | yes | + e2e / platform | detailed |

> Record the lane in the worklog frontmatter and in the **Lane** field of the story, so you
> can later grep which work was at which risk level.
