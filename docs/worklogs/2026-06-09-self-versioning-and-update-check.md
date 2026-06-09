---
date: 2026-06-09
who: both
lane: normal
input_type: framework-improvement
outcome: completed
touched: [ADR-0003, wiki/versioning]
files_changed:
  - docs/framework/VERSION
  - docs/framework/CHANGELOG.md
  - docs/decisions/0003-self-versioning-and-update-check.md
  - docs/wiki/versioning.md
  - .claude/skills/riverflow-update/SKILL.md
  - README.md
friction: none
---

# Worklog: 2026-06-09 — riverflow self-versioning + update check

## Done

- Added `docs/framework/VERSION` (`0.1.0`) as the single source of truth, namespaced under the
  framework dir so it travels with the install and never collides with a host product's own version.
- Added `docs/framework/CHANGELOG.md` (Keep-a-Changelog / SemVer) so the update check can show
  *what changed*, not just a number.
- New `riverflow-update` skill: find local VERSION → shallow `git clone` upstream into a temp dir →
  read remote VERSION → `sort -V` semver compare → show changelog delta → on confirmation copy
  framework/templates/skills over (never the user's docs instances) → clean up temp.
- Recorded the design in [ADR-0003](../decisions/0003-self-versioning-and-update-check.md) and the
  as-built in [docs/wiki/versioning.md](../wiki/versioning.md).
- Updated README: repo structure, install prompt (now copies `riverflow-update` too), new
  "Versioning & updates" section.

## In-place decisions

- Version at `docs/framework/VERSION`, not repo root — avoids collision with a host product's own
  `VERSION` and travels with the already-copied framework dir. (Full rationale → ADR-0003.)
- Update mechanism = clone-temp-compare, no registry — keeps the "pure Markdown, no infra" stance.
- Initial version `0.1.0`, with the versioning mechanism itself counted as part of that baseline.

## Proof

- Verified live: local VERSION reads `0.1.0`; semver verdicts correct for remotes
  `0.1.0` (up-to-date), `0.2.0` / `1.0.0` (update), `0.0.9` (local-ahead); shallow clone of the
  real GitHub repo succeeds and the temp dir is removed.

## Still open

- [ ] Commit + push so upstream carries `docs/framework/VERSION` — until then the live check sees
      no remote VERSION (handled as pre-versioning).
- [ ] Future: a `MAJOR` bump should ship a short migration note alongside the changelog entry.

## Next step

Push the change, then run "check for riverflow updates" from a real installed repo to confirm the
end-to-end report renders.
