---
topic: <slug>                      # topic/capability, e.g. rbac, billing, auth
updated: YYYY-MM-DD                 # last time this page was edited
verified_against_code: YYYY-MM-DD   # last time the content was checked against real code
source_specs: []                   # specs that fed this page: [spec-0007, spec-0012]
related_adrs: []                   # foundational ADRs: [ADR-0004]
---

# Wiki: <capability name>

> A **living** as-built page: it describes how this capability *currently* works, not how it
> *should* work (that lives in the spec). Each ship touching this capability **overwrites it in
> place** here. The frontmatter is the freshness signal — a stale `verified_against_code` means treat with suspicion.

## Summary

One sentence: what this capability is, what it solves.

## How it works now

Actual behavior right now — the main flow, the roles/states, the constraints in force.
Written in the present tense. Enough for a later agent to understand without reading the code yet.

## Where the code is

The entry points and the key files/modules. Update when the structure changes.

| Part | Path |
| --- | --- |
| <entry point> | `<path>` |
| <core logic> | `<path>` |

## Foundational decisions

Why it is the way it is — link to ADRs instead of copying the reasoning.

- [ADR-NNNN](../decisions/NNNN-....md) — <what it settled>

## Extending it & pitfalls

How to add/change safely, and where it's easy to go wrong. (Drop this section if there is nothing worth noting yet.)

- <note>
