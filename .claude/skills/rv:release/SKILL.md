---
name: rv:release
description: CORE-MAINTAINER ONLY — cut a new riverflow framework version. Bumps docs/framework/VERSION and adds a matching docs/framework/CHANGELOG.md entry (deciding MAJOR/MINOR/PATCH from the changes since the last release, per the framework's own SemVer rules). This skill is NOT part of an install — it lives only in the riverflow core repo and is never copied into a project that installs riverflow. Use when maintaining the riverflow repo itself and you want to release a new version, e.g. "rv:release", "rv:release 0.2.0", "bump riverflow version", "cut a riverflow release".
---

# rv:release

**Core-maintainer skill.** It raises the version of riverflow *itself* — only run it inside the
riverflow **core** repo, never in a project that merely installs riverflow. It is the counterpart
to `rv:update-version` (which is the *consumer* side: pull the latest into an install).

> Not shipped on install. The install prompt and `rv:update-version` copy only `rv:recap`,
> `rv:brainstorm`, and `rv:update-version`. This skill is deliberately excluded — keep it that
> way (see "Boundaries").

## When to use

Maintaining the riverflow repo and ready to publish a new version: "rv:release",
"rv:release 0.2.0", "bump riverflow version", "cut a release".

## Procedure

### 1. Confirm this is the riverflow CORE repo
Bumping `VERSION` only makes sense in core — in an installed copy the version reflects *what was
installed*, so never bump it there. Confirm at least one core marker before doing anything:

- the git remote is `phidn/riverflow` (`git remote -v`), **or**
- the repo root README is riverflow's own ("riverflow is not code … conventions + document
  templates") and `docs/framework/` holds the framework source `00`–`06`.

If it looks like an *installed* repo (a host product that copied riverflow in), stop and tell the
user this skill is core-only — they probably want `rv:update-version` instead.

### 2. Read the current version + the unreleased changes
- Current version: `docs/framework/VERSION`.
- Gather what changed since the last release: `git log <last-tag-or-recent>..HEAD --oneline`, the
  working tree (`git status`), and any notes the user gave. Summarize the changes.

### 3. Decide the bump (or take the explicit version)
- If the user gave an explicit version ("rv:release 0.2.0"), use it (validate it is greater than
  current, semver-shaped).
- Otherwise pick MAJOR / MINOR / PATCH **per the rules at the top of `CHANGELOG.md`**:
  - MAJOR — a breaking convention change (renamed/removed artifact type, changed layout or
    frontmatter contract, **renamed a skill trigger** that consumers rely on).
  - MINOR — a new artifact type / skill / template / framework doc, backward compatible.
  - PATCH — wording / clarification / typo, no behavior change for an installed repo.
- State the proposed version + why, in one line. Bias to acting on the recommendation (the user
  adjusts after) — but if a change looks breaking and the user asked for a mere PATCH, say so.

### 4. Write the bump
- Overwrite `docs/framework/VERSION` with the new version (single line, trailing newline).
- Prepend a new section to `docs/framework/CHANGELOG.md` **above** the previous version's section:
  ```
  ## [<new>] — <today>

  ### Added / Changed / Fixed / Removed   (only the headings that apply)
  - <one line per notable change, consumer-facing>
  ```
  Date = **today** (use the real current date; ask if unsure — never invent one).
- Keep entries short and consumer-facing — describe the effect on an installed repo, not the diff.

### 5. Offer to commit + tag (don't do it unsolicited)
- Propose: `git commit` the bump and `git tag v<new>`. Do **not** push on your own — ask.
- Use `gitx`, not bare `git`, for any GitHub operations. If on the default branch, branch first.

## Boundaries

- **Core-only, never installed.** Do not add `rv:release` to the README install prompt or to the
  copy list in `rv:update-version`. Those propagate only `rv:recap` + `rv:brainstorm` +
  `rv:update-version`. If you ever extend the update/install copy logic, keep `rv:release` out of it.
- Bump **only** in the core repo (step 1). Never bump `VERSION` in an installed/host repo.
- VERSION and CHANGELOG move together — never bump one without the other, or the consumer-side
  `rv:update-version` check will show a number with no explanation.
- Don't rewrite released CHANGELOG sections; add a new one on top.
