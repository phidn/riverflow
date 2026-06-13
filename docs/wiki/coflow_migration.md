# Migrate from coflow

Already on **coflow** (riverflow's predecessor)? Paste this prompt — it upgrades the framework in place and **leaves all your own artifacts untouched**.

```
Clone https://github.com/phidn/riverflow into a temp dir. My project currently
follows coflow — migrate it to riverflow. Replace `docs/framework/` and
`docs/templates/` with riverflow's (this adds `docs/framework/VERSION` +
`CHANGELOG.md` — the self-versioning layer coflow lacked). Copy its
`.claude/skills/rv:recap/` and `.claude/skills/rv:update-version/` into my
`.claude/skills/`, then delete the old `skills/coflow-capture/` (`rv:recap` is its
successor). Replace `AGENTS.md` with riverflow's, and re-point the `CLAUDE.md`
symlink at it if one exists. DO NOT touch anything under
`docs/{decisions,stories,specs,wiki,backlogs,worklogs}/` — those are my own
artifacts and must survive the migration unchanged. Read riverflow's README +
AGENTS.md first, then summarize what changed (skills renamed, versioning added,
AGENTS.md swapped) back to me.
```

What moves: the framework/templates get the upstream version with `VERSION` + `CHANGELOG`, the capture skill `coflow-capture` is replaced by `rv:recap` (plus the new `rv:update-version` check), and `AGENTS.md` is refreshed. What stays put: every file under `docs/decisions/`, `docs/stories/`, `docs/specs/`, `docs/wiki/`, `docs/backlogs/`, and `docs/worklogs/` — your real work is never overwritten. After migrating, run `/rv:update-version` anytime to stay current.
