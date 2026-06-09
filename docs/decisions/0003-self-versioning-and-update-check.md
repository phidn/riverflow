# ADR-0003: riverflow self-versioning + update check

- **Status:** accepted
- **Date:** 2026-06-09
- **Decided by:** philo (human)
- **Related:** [docs/wiki/versioning.md](../wiki/versioning.md), `riverflow-update` skill, [04-conventions](../framework/04-conventions.md)

## Context

riverflow is copied into a host project (framework docs + templates + skills), then drifts: the
host's copy ages while the upstream framework keeps improving. There was no way for an installed
repo to know it was out of date. We want (1) a version stamp on the framework and (2) a skill that
tells the user when a newer version exists upstream and offers to update.

Two sub-questions had real trade-offs:

1. **Where does the version live?** A host project frequently has its **own** product version at
   root `VERSION` / `package.json`. riverflow's version must not be confused with, or collide
   with, the host's.
2. **How does the check learn the latest version?** There is no package registry — riverflow is
   "pure Markdown, no infrastructure" (README maturity stance). The source of truth is the Git repo
   itself.

## Options

### Option A — version at repo root `VERSION`
- Pros: conventional location, easy to find.
- Cons: **collides** with the host product's own root `VERSION`; the install would either
  overwrite the host's file or be skipped. Ambiguous which version a reader is looking at.

### Option B — version inside the framework dir (`docs/framework/VERSION`)
- Pros: namespaced to riverflow, no collision with the host's version. **Travels automatically** —
  the install already copies `docs/framework/`, so no extra copy step. One obvious home for a
  reader ("riverflow's version is under riverflow's framework").
- Cons: slightly less discoverable than root; mitigated by a README pointer.

### Check mechanism — registry vs. clone-temp-compare
- A package registry / API endpoint would mean infrastructure — rejected, violates the
  no-tooling stance.
- **Shallow `git clone` into a temp dir, read its `VERSION`, semver-compare, then delete the temp
  dir.** No infra, read-only, works for anyone with `git`.

## Decision

Chose **Option B** (version at `docs/framework/VERSION`) plus the **clone-temp-compare** check,
because the version travels with the existing install copy and stays namespaced away from the
host's own version, and the check needs no infrastructure beyond `git`.

A `docs/framework/CHANGELOG.md` accompanies the version so the update skill can show *what changed*
between the installed version and upstream, not just that a newer number exists.

## Consequences

- Positive: an installed repo can self-report staleness with one skill run; zero infra; the
  version ships wherever the framework ships.
- Trade-offs we accept: the check requires network + `git` at run time; a forgotten `VERSION`/
  `CHANGELOG` bump on a framework change makes the check lie — so **bumping both is part of every
  framework-improvement change** (recorded in the wiki).
- Opens up: signed releases / git tags later, if we ever want them, without changing where the
  version lives.
