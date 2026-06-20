# ADR-0006: `install:` frontmatter marker is the single source of truth for which skills ship

- **Status:** accepted
- **Date:** 2026-06-20
- **Decided by:** philoha
- **Related:** [plan-0005](../plans/0005-rv-playbook-skill.md) (the rv:playbook skill that exposed the friction), [backlog-0003](../backlogs/0003-rv-playbook-graduate-core.md)

> An ADR is for a choice that **outlives one change** — every future skill added to riverflow
> depends on this one.

## Context

"Which skills ship when a project installs riverflow" was duplicated as hand-maintained name lists
across five places: the README dir tree, the README fresh-install prompt, the README migrate
prompt, the README prose paragraph, the `rv:update-version` copy block, and the `rv:release`
boundaries. Adding `rv:playbook` revealed the cost — a new skill is silently missing from installs
until someone remembers to edit every list, and the lists drift out of sync. We need one source of
truth so adding a skill never means editing the install machinery.

## Options

### Option A — `install:` field in each SKILL.md frontmatter
- Pros: source of truth lives **with** the skill; adding a skill = set one field, touch nothing
  else; prompts/copy logic become generic and stable forever; self-describing; greppable.
- Cons: the install/update logic must read each skill's frontmatter (a tiny loop); relies on an
  extra frontmatter key (ignored by the Claude Code skill loader).

### Option B — a manifest file (`docs/framework/skills.md`)
- Pros: one human-browsable catalog; fits the "everything is a readable artifact" ethos.
- Cons: still a separate list that can drift from the actual skill dirs; one more file to keep in
  step with reality; the marker isn't co-located with the thing it describes.

### Option C — install-all-except-core-marked
- Pros: simplest prompt; new skills install by default.
- Cons: a not-yet-ready skill ships by accident; less explicit; inverts the safe default.

## Decision

Chose **Option A** — a boolean `install:` field in each SKILL.md frontmatter (`true` = shipped on
install, `false` = core-only) is the single source of truth, because the marker travels with the
skill so adding one never requires editing the install prompt, `rv:update-version`, or `rv:release`.

## Consequences

- Positive: README prompts + prose, the `rv:update-version` copy block, and the `rv:release`
  boundaries are now generic ("copy every skill with `install: true`"); the install set self-maintains.
- Trade-offs we accept: the copy logic greps frontmatter (`^install:\s*true`); a malformed/missing
  marker means a skill is treated as **not** shipped (safe default — fail closed).
- Opens up: future skills just declare `install:` and are picked up automatically. Current values —
  `rv:brainstorm`, `rv:recap`, `rv:playbook`, `rv:update-version` → `true`; `rv:release` → `false`.
- Closes off: hand-maintained skill name lists in distribution surfaces — they must not return.
