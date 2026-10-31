# SYSTEM Protocol Versions

This directory is the visible release archive and draft area for llmfs `SYSTEM.md` protocol templates.

There is intentionally no `SYSTEM.md` in this directory. A managed filesystem is governed only by the nearest installed `SYSTEM.md` in its own directory tree.

## Current released version

- Latest released: [SYSTEM.v10.md](SYSTEM.v10.md)
- Current install template: [SYSTEM.md](../llmfs/templates/SYSTEM.md)
- Draft for next release: [SYSTEM.next.md](SYSTEM.next.md)

## Rules

1. Released protocol files are named `SYSTEM.vX.md`.
2. A released `SYSTEM.vX.md` is immutable after publication.
3. Do not edit a released `SYSTEM.vX.md`, even for small corrections.
4. All future protocol changes must be made in `SYSTEM.next.md` first.
5. Releasing `SYSTEM.next.md` creates a new `SYSTEM.vX.md`.
6. The same finalized content must also be copied to `../llmfs/templates/SYSTEM.md` for installation.
7. `protocol/` must not contain `SYSTEM.md`.
8. `SYSTEM.next.md` is a draft and must not be used for normal initialization unless explicitly requested.
9. `SYSTEM` files must not add metadata header blocks; version and release date belong only in the first Markdown heading.

## Title convention

Released protocol files and the current install template use this first-heading format:

```markdown
# System Protocol vX YYYY-MM-DD
```

Example:

```markdown
# System Protocol v0 2026-04-28
```

`SYSTEM.next.md` uses this draft heading:

```markdown
# System Protocol next
```

Do not add extra metadata headers such as `Version:`, `Status:`, `Released:`, or `Canonical filename:` to any `SYSTEM` file.

## Released versions

| Version | File | Release date | Notes |
| --- | --- | --- | --- |
| v0 | [SYSTEM.v0.md](SYSTEM.v0.md) | 2026-04-28 | Initial released protocol snapshot. |
| v1 | [SYSTEM.v1.md](SYSTEM.v1.md) | 2026-04-29 | Streamlined and hardened protocol with bootstrap completion, existing-content adoption, trusted nested authority boundaries, and stronger prompt-injection safeguards. |
| v2 | [SYSTEM.v2.md](SYSTEM.v2.md) | 2026-04-29 | Clarified language policy so `LOG.md` subjects and bodies use the user's language while heading actions may remain stable English slugs. |
| v3 | [SYSTEM.v3.md](SYSTEM.v3.md) | 2026-05-03 | Clarified `LOG.md` entries must be appended to the physical file end; clarified wikilink alias policy to prefer bare targets and avoid redundant aliases. |
| v4 | [SYSTEM.v4.md](SYSTEM.v4.md) | 2026-06-27 | Added task-mode selection, scoped retrieval discipline, LOG lifecycle, current-state / handoff triggers, maintenance/search-surface checks, temporary artifact lifecycle, and non-regression constraints while keeping package release rules out of ordinary managed filesystem runtime protocol. |
| v5 | [SYSTEM.v5.md](SYSTEM.v5.md) | 2026-07-03 | Added minimal scoped subagent delegation guidance under task modes: use only when materially helpful, treat subagents as agents under the same applicable `SYSTEM.md`, prefer parallel read-only delegation, path-scope mutating subtasks, and verify key subagent claims before reporting or preserving durable results. |
| v6 | [SYSTEM.v6.md](SYSTEM.v6.md) | 2026-07-22 | Added narrowly scoped proactive experience capture during Discussion when reusable guidance, long-term value, scope, and target are clear; Retrieval remains read-only, and the rule avoids separate retrospectives, filesystem-wide scans, unrelated maintenance, and duplicate reporting mechanisms. |
| v7 | [SYSTEM.v7.md](SYSTEM.v7.md) | 2026-07-22 | Added product-independent external skill packages as visible ordinary-file dependencies with semantic placement, explicit task/runbook/workflow references, source and provenance checks, archive target safety, scoped method authorization, separate storage/binding/executability states, cross-Agent compatibility limits, and filesystem-local experience separation. |
| v8 | [SYSTEM.v8.md](SYSTEM.v8.md) | 2026-07-28 | Added product-independent workflow credential authority, retrieval exclusion, disclosure and external-side-effect boundaries, four-state readiness, same-user isolation limits, migration/cleanup safeguards, and optional external-store bindings; compacted the full protocol while preserving reviewed semantic and safety behavior. |
| v9 | [SYSTEM.v9.md](SYSTEM.v9.md) | 2026-09-10 | Required local commits for file-changing tasks without repeated confirmation and reporting of commit outcomes; aligned task closeout references in §4 and §9. |
| v10 | [SYSTEM.v10.md](SYSTEM.v10.md) | 2026-10-31 | Replaced wikilinks with standard Markdown links, document-relative local targets, actual extensions, necessary filename encoding, and affected-link verification after moves/renames; aligned §§5/9/17. |

## Draft

- [SYSTEM.next.md](SYSTEM.next.md) — mutable next-cycle draft following the v10 release, with the same body as [SYSTEM.v10.md](SYSTEM.v10.md) except for the draft heading. Future changes require a new candidate validation and independent freeze review cycle; they must not modify released v10. Operational credential validation remains not performed, a real pilot remains optional post-release validation, and operational cross-Agent compatibility remains not established.

## Release process

1. Edit [SYSTEM.next.md](SYSTEM.next.md) as a mutable draft.
2. Anchor and review the exact candidate. Satisfy or formally waive candidate-specific validation gates with honest status and narrowed claims, then pass independent freeze review before release. Record candidate hashes and validation/review results in the PR description or release commit message, not in per-release review sections in this index.
3. Choose the next version number and release date only after the freeze gate passes.
4. Set the finalized release heading to `# System Protocol vX YYYY-MM-DD`.
5. Copy the finalized content to `SYSTEM.vX.md` and copy the same content to [SYSTEM.md](../llmfs/templates/SYSTEM.md).
6. Update the repository `README.md` version structure and, when the install template changes, update the package version in `llmfs/SKILL.md`.
7. Reset or continue [SYSTEM.next.md](SYSTEM.next.md) as the next mutable draft with the heading `# System Protocol next`, and update this index.
8. Verify the immutable release, install template, draft baseline, index links, and package metadata before committing the inner repository release.
9. After the inner release commit, update the outer submodule pointer, design status, visible indexes, and `LOG.md` with task-scoped staging.
10. Treat package release and installation into any active managed-filesystem root as separate stages; do not modify a root `SYSTEM.md` without separate authorization.
11. If release fails before the verified inner release commit, clear partial release changes and return to the frozen candidate. If failure occurs after that commit, preserve immutable `SYSTEM.vX.md`; repair outer integration or publish a later corrective version rather than modifying/deleting the release.
