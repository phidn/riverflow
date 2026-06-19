---
date: YYYY-MM-DD
who: agent | human | both
lane: tiny | normal | high-risk
input_type: change-request   # input type at intake: new-brief | story-slice | change-request | initiative | maintenance | framework-improvement
outcome: completed | partial | blocked | failed
touched: []            # ids of artifacts touched: [story-0001, plan-0001, ADR-0002]
files_changed: []      # paths of files changed
friction: none         # "none" if smooth; or a short note on the snag / missing docs / unclear proof
---

# Worklog: YYYY-MM-DD — <session title>

> The frontmatter above is the part machines/agents read fast (greppable). The part below is for humans.
> Lane `tiny` needs only `date` + `outcome` + the "Done" section. Lane `high-risk` fills it all.

## Done

- <item 1>
- <item 2>

## In-place decisions

Small choices locked during the session (record them in the plan's Decisions section; choices that outlive the change get their own ADR).

- <decision> — because <reason>

## Still open

- [ ] <unfinished work / hanging question>
- [ ] <unfinished work / hanging question>

## Next step

Where the next human/agent should start.
