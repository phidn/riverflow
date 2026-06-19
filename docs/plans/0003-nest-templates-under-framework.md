# Plan-0003: Nest templates under framework

- **Status:** approved
- **Related brief:** —
- **Related stories:** —
- **Related ADRs:** —

> Thin plan, lane `normal`. Approved verbally in-session before execution.

## Summary

Relocate `docs/templates/` to `docs/framework/templates/` so the whole riverflow "core"
(rules + the blank forms those rules define) lives under one root. Today `docs/` mixes two
framework-owned directories (`framework/`, `templates/`) in among the six user-owned artifact
directories, blurring the line between "riverflow itself" and "your work". After the move the
invariant is clean: **`docs/framework/` = riverflow (don't touch); everything else in `docs/` =
your artifacts.**

Side benefit: install and `rv:update-version` copy steps collapse from two `cp -R` (framework +
templates) to one, because templates now ride inside `docs/framework/`.

## Technical design

- `git mv docs/templates docs/framework/templates` (one move, history preserved).
- Update **live** references from `docs/templates/...` → `docs/framework/templates/...`:
  skills (`rv:brainstorm`, `rv:recap`, `rv:update-version`), framework docs (`00-overview.md`,
  `02-lifecycle.md`, `03-artifacts.md`), `AGENTS.md`, `README.md` (tree diagram + install +
  migration prompts), ADR "Related" links (`0001`, `0002`).
- **Historical** references are left verbatim — worklogs, `CHANGELOG.md` prior entries, and the
  frozen `plans/0001` proof command are point-in-time records.
- `rv:update-version` copy logic: drop the separate `cp -R docs/templates`; add a one-time
  relocation step for existing installs (move their old `docs/templates/` into
  `docs/framework/templates/` and remove the orphaned dir) gated on user confirmation.
- Version: **MINOR → 0.6.0** (pre-1.0 breaking layout change, same treatment as the 0.3.0
  spec→plan rename) + a CHANGELOG entry with the migration note.

## Decisions

- Nest as `docs/framework/templates/` (subfolder), **not** flat `framework/template_*.md` — flat
  would mix fill-in forms among the numbered `00-…06-` chapters and re-clutter the very folder we
  are cleaning. Keeps the prose-rules vs blank-form boundary.
- MINOR not MAJOR, following the framework's own pre-1.0 precedent (0.3.0 note: "breaking lands
  in MINOR").

## Implementation steps

- [x] `git mv docs/templates docs/framework/templates`
- [x] Update live refs in skills, framework docs, AGENTS.md, README.md, ADR links
- [x] Rework `rv:update-version` copy + add install-relocation step
- [x] Bump VERSION → 0.6.0 + CHANGELOG entry
- [x] Proof: grep shows no live `docs/templates/` refs remain

## Acceptance criteria

- [x] `docs/templates/` no longer exists; templates live at `docs/framework/templates/`
- [x] No **live** reference points at the old path (historical records may)
- [x] `rv:update-version` copies framework once and relocates an old install's templates

## Proof

| Tier | Needed? | Evidence |
| --- | --- | --- |
| Unit | no | — |
| Integration | yes | `grep -rn "docs/templates/" --include="*.md" .` returns only historical files (worklogs, CHANGELOG, plans/0001) |
| E2E | no | — |

## Out of scope

- Renaming or restructuring the numbered framework chapters.
- Touching any user-owned artifact directory.
