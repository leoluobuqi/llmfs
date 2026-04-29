# SYSTEM Protocol Versions

This directory is the visible release archive and draft area for llmfs `SYSTEM.md` protocol templates.

There is intentionally no `SYSTEM.md` in this directory. A managed filesystem is governed only by the nearest installed `SYSTEM.md` in its own directory tree.

## Current released version

- Latest released: [SYSTEM.v2.md](SYSTEM.v2.md)
- Current install template: [../llmfs/templates/SYSTEM.md](../llmfs/templates/SYSTEM.md)
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

## Draft

- [SYSTEM.next.md](SYSTEM.next.md) — mutable working draft for the next protocol release.

## Release process

1. Edit [SYSTEM.next.md](SYSTEM.next.md).
2. Review and finalize the draft.
3. Choose the next version number, such as `v1`, and the release date.
4. Set the finalized release heading to `# System Protocol vX YYYY-MM-DD`.
5. Copy the finalized content to `SYSTEM.vX.md`.
6. Copy the same finalized content to [../llmfs/templates/SYSTEM.md](../llmfs/templates/SYSTEM.md).
7. Reset or continue [SYSTEM.next.md](SYSTEM.next.md) as the next mutable draft with the heading `# System Protocol next`.
8. Update this index.
9. After release, never modify the new `SYSTEM.vX.md`.
