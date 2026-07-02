---
topic: versioning
updated: 2026-06-19
verified_against_code: 2026-06-19
---

# Versioning & updates

How riverflow stamps its own version and how an installed repo learns it is out of date.
Decision: [ADR-0003](../decisions/0003-self-versioning-and-update-check.md).

## Where the version lives

- **Single source of truth:** `docs/framework/VERSION` — one line, semver (e.g. `0.1.0`).
- It sits **inside** `docs/framework/` on purpose: the install copies that directory, so the
  version **travels with the install** automatically, and it stays namespaced away from the host
  project's own `VERSION` (a host product often has its own root version — these must not collide).
- `docs/framework/CHANGELOG.md` — Keep-a-Changelog format, SemVer. Records *what changed* so the
  update check can show a delta, not just a number.

## SemVer meaning for the framework

- **MAJOR** — a breaking convention change (renamed/removed artifact type, changed file layout or
  frontmatter contract) an installed repo must migrate to.
- **MINOR** — a new artifact type / skill / template / framework doc, backward compatible.
- **PATCH** — wording/clarification/typo, no behavior change.

## The update check (`rv:update-version` skill)

Flow: find local `VERSION` → shallow `git clone` upstream into a temp dir → read its `VERSION` →
`sort -V` semver compare → show the changelog delta → on confirmation, copy framework (templates
ride inside it at `docs/framework/templates/`) + skills over (never the user's `docs/` instances),
relocating an old install's orphaned `docs/templates/` → delete the temp dir.

- Local version is searched at `docs/framework/VERSION`, then `framework/VERSION`, then `VERSION`
  (standard + embedded install shapes). Missing → treated as `0.0.0` (pre-versioning install).
- Upstream source of truth is the Git repo itself — no registry, no infrastructure (matches the
  README "pure Markdown, no infra" stance).
- Read-only by default; the only write step (applying the update) requires explicit user
  confirmation and never overwrites the user-authored docs instances or (blindly) `CLAUDE.md`.

## Maintenance rule (don't let the check lie)

Every **framework-improvement** change bumps `docs/framework/VERSION` **and** adds a
`CHANGELOG.md` entry, in the same change. A framework edit that forgets the bump makes installed
repos believe they are current when they are not.
