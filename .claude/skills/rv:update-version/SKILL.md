---
name: rv:update-version
install: true   # shipped on install — see ADR-0006 (install: is the single source of truth)
description: Check whether the installed riverflow framework is out of date against the latest on GitHub, and offer to update. Canonical trigger is the prefix "rv:update-version". Clones https://github.com/phidn/riverflow into a temp dir, reads its docs/framework/VERSION, semver-compares it with the version installed in this repo, shows the changelog delta, and — only if the user confirms — copies the newer framework (templates included) and skills over (never touching the user's own docs instances). Use when the user types "rv:update-version" or otherwise asks to check/apply riverflow updates (e.g. "is riverflow up to date?", "update riverflow", "/rv:update-version").
---

# rv:update-version

Tells an installed repo whether its riverflow framework is **behind upstream**, shows **what
changed**, and — on confirmation — applies the update. The source of truth for "latest" is the
Git repo itself (no registry, no infra). See [ADR-0003](../../../docs/decisions/0003-self-versioning-and-update-check.md).

> The version lives at `docs/framework/VERSION`, namespaced under the framework dir so it travels
> with the install and never collides with the host project's own version.

## When to use

Canonical trigger: the prefix **`rv:update-version`**. Also fires on natural phrasing — "check for
riverflow updates", "is riverflow up to date?", "update riverflow", "/rv:update-version". Do NOT run
on every session — only when triggered.

## Procedure

### 1. Find the installed version

Look for the local `VERSION` file in these candidate paths, first match wins (covers the standard
and embedded install shapes — same detection style as `rv:recap`):

1. `docs/framework/VERSION`  (standard riverflow install)
2. `framework/VERSION`       (embedded install)
3. `VERSION`                 (last resort)

If none exists → this repo predates self-versioning. Treat the local version as `0.0.0` and tell
the user their install is from before versioning existed (so any upstream version is an update).

Read the file; trim whitespace. Call it `LOCAL`.

### 2. Fetch the latest version from GitHub

Shallow-clone into a temp dir, read the remote `VERSION`, then clean up. Do not leave the temp dir
behind.

```bash
TMP="$(mktemp -d)"
git clone --depth 1 https://github.com/phidn/riverflow "$TMP" >/dev/null 2>&1 \
  || { echo "clone-failed"; }
REMOTE="$(tr -d '[:space:]' < "$TMP/docs/framework/VERSION" 2>/dev/null)"
echo "REMOTE=$REMOTE"
```

Keep `$TMP` around until step 4 (you reuse its files to show the changelog and to apply the
update). If the clone fails (no network / no `git`), report that and stop — do not guess.

### 3. Compare (semver) and report

```bash
LATEST="$(printf '%s\n%s\n' "$LOCAL" "$REMOTE" | sort -V | tail -n1)"
```

- `LOCAL` = `REMOTE` → **up to date.** Report the version and stop (delete `$TMP`).
- `LOCAL` = `REMOTE` is false **and** `LATEST` = `REMOTE` → **update available** (`LOCAL → REMOTE`).
- otherwise (`LATEST` = `LOCAL`, versions differ) → **local is ahead** of upstream (a dev/unreleased
  copy). Report it, no action needed.

When an update is available, show the user **what changed**: read the upstream changelog at
`$TMP/docs/framework/CHANGELOG.md` and quote the entries **newer than `LOCAL`** (every section
above the `## [LOCAL]` heading). This is the value — the version number alone is not enough.

Then read out a short block, e.g.:

```
riverflow: 0.1.0 installed → 0.2.0 available

What's new:
  0.2.0 — Added: retro template; Changed: intake Output block adds a Proof line
  ...

Update now? (copies framework + templates + skills; leaves your docs/ instances untouched)
```

### 4. Apply the update — only on explicit confirmation

Updating is the irreversible part. **Ask first; do not auto-apply.** When the user confirms, copy
**only** the framework (templates ride inside it at `docs/framework/templates/`) and the shipped
skills from `$TMP` — never the example instances
(`docs/decisions/`, `docs/stories/`, `docs/plans/` — or `docs/specs/` in a pre-0.3.0 install —
`docs/wiki/`, `docs/backlogs/`, `docs/worklogs/`), which hold the user's own work.

> Crossing 0.3.0: the framework renamed `spec` → `plan` (`docs/specs/` → `docs/plans/`, template
> `spec.md` → `plan.md`) and dropped the `task` type. After copying, point the migration out and
> offer to `git mv docs/specs docs/plans` and remove the stale `docs/templates/{spec,task}.md` —
> only with the user's confirmation, since it touches their instances. See the 0.3.0 CHANGELOG
> entry for the full migration note.

> Crossing 0.6.0: templates moved from the top-level `docs/templates/` to `docs/framework/templates/`.
> The copy below brings the new templates in *inside* `docs/framework/`, so the old top-level
> `docs/templates/` is left orphaned — the cleanup line removes it. Templates are riverflow-owned
> (the user copies but never authors them), so this is safe; only run it once, when the old dir
> still exists.

```bash
# standard install layout — adjust the dest prefix for an embedded layout
cp -R "$TMP/docs/framework/."            docs/framework/   # templates ride along inside framework/templates/

# copy only the skills riverflow marks shippable — `install: true` in SKILL.md frontmatter
# is the single source of truth (ADR-0006); `install: false` (e.g. rv:release) is skipped.
for d in "$TMP"/.claude/skills/*/; do
  grep -qE '^install:[[:space:]]*true\b' "$d/SKILL.md" 2>/dev/null || continue
  name=$(basename "$d")
  mkdir -p ".claude/skills/$name"
  cp -R "$d." ".claude/skills/$name/"
done

# one-time relocation (crossing 0.6.0): the old top-level templates dir is now obsolete
[ -d docs/templates ] && rm -rf docs/templates
```

- This overwrites the framework docs (templates included) and the riverflow skills (the parts the
  user does not author) and bumps `docs/framework/VERSION` to `REMOTE` as a side effect of copying it.
- AGENTS.md: do **not** blindly overwrite — the host may have customized it. If the upstream
  AGENTS.md changed, tell the user and offer to show a diff rather than clobbering theirs.
- If the changelog lists a **MAJOR** bump (a breaking convention change), call that out explicitly
  and point the user at the relevant changelog section before/after applying — they may need to
  migrate existing artifacts.

### 5. Clean up + confirm

```bash
rm -rf "$TMP"
```

Summarize: old version → new version, which directories were refreshed, and anything (AGENTS.md,
a MAJOR migration) left for the user to handle. Offer to write a `worklog` recording the update
(lane `tiny`, `input_type: maintenance`). Don't commit/push on your own — ask.

## Boundaries

- **Read-only check by default.** The version check never modifies anything; only step 4 writes,
  and only after the user confirms.
- **Never overwrite the user's docs instances** — only the framework (templates included) and the
  skills are riverflow's to update.
- Always delete the temp clone, even on failure.
- If `git` or the network is unavailable, say so plainly — do not fabricate a version.
