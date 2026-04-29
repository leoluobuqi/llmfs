---
name: llmfs
description: Use when a user asks to initialize, adopt, check, or update a directory as an LLM-managed self-describing filesystem. Install or update only SYSTEM.md and AGENTS.md, then hand off to the target SYSTEM.md.
version: 0.2.0
---

# llmfs

## When to use this skill

Use this skill when the user asks to make a directory into an LLM-managed self-describing filesystem, for example:

- "Initialize this directory as an LLM-managed filesystem."
- "Adopt this existing folder as llmfs."
- "Check whether this directory has llmfs bootstrap files."
- "Update this llmfs directory to the current packaged SYSTEM template."

This skill is only for the bootstrap layer. It installs local protocol entry points and then hands off to the target directory's `SYSTEM.md`.

## What this skill does

It installs, updates, or checks exactly these local protocol entry points:

A self-describing filesystem may contain arbitrary file types, but its durable protocol, organization, routing, and retrieval aids must be visible ordinary files such as `SYSTEM.md`, `README.md`, `INDEX.md`, and `LOG.md`, not hidden databases, vector stores, embedding caches, or RAG indexes.

```text
<target>/
├── SYSTEM.md
└── AGENTS.md
```

It does not create `README.md`, `INDEX.md`, `LOG.md`, `inbox/`, or any other filesystem structure. Those are created later by the target directory's `SYSTEM.md` after handoff.

## Authority Boundary

This skill only installs local protocol entry points.

After bootstrap, the target directory's `SYSTEM.md` is the local operating authority.

Never use this skill to override, replace, or reinterpret an existing `SYSTEM.md` without explicit human approval.

If this skill conflicts with the target directory's `SYSTEM.md`, follow the target directory's `SYSTEM.md`.

## How to use this skill

First decide the mode from the user's request:

- **init / bootstrap** — target directory is new, empty, or the user explicitly wants llmfs installed there.
- **adopt** — target directory already has content but no `SYSTEM.md`.
- **check** — user only wants to know whether llmfs bootstrap is present.
- **update-protocol** — target already has a trusted `SYSTEM.md`, and the user wants to update the bootstrap protocol entry points to this skill package's current templates.

If the request is ambiguous, prefer the safest interpretation:

- never replace an existing `SYSTEM.md` without explicit approval;
- never move or reorganize existing files as part of this skill;
- install only `SYSTEM.md` and `AGENTS.md` before handoff.

## Preflight

Before writing to a target directory:

1. Resolve the target path.
2. Check whether the target already has `SYSTEM.md`.
3. If `SYSTEM.md` exists and the mode is not update-protocol, stop and report already governed unless the human explicitly approves replacement.
4. If the target is inside an existing `SYSTEM.md`-governed tree, read and follow the governing `SYSTEM.md`.
5. If creating a nested `SYSTEM.md` boundary inside an existing governed tree, confirm that the human wants a nested authority boundary.
6. Check whether the target is a git repository.
7. If the target is not a git repository, ask whether to run `git init`; recommend yes because git provides local rollback checkpoints.
8. Ask before dangerous targets such as filesystem root, home directory, system directories, or workspace parents.

## Bootstrap / Init

1. Run preflight.
2. Create the target directory if it does not exist and the human intent is clear.
3. If the target is not a git repository, ask whether to run `git init`; recommend yes.
4. If the human agrees, run `git init`.
5. Copy `templates/SYSTEM.md` to `<target>/SYSTEM.md`.
6. Copy or minimally merge `templates/AGENTS.md` to `<target>/AGENTS.md`.
7. Verify `SYSTEM.md` and `AGENTS.md` exist.
8. Stop using this skill as the operating authority.
9. Read `<target>/SYSTEM.md`.
10. By default, continue initialization by following `<target>/SYSTEM.md`, unless the human explicitly asked only to install protocol entry points.
11. If the target is a git repository, follow `<target>/SYSTEM.md` git policy for committing files changed during the task.

## Adopt Existing Directory

Use the same workflow as bootstrap, with extra care:

- do not move existing files;
- do not overwrite ordinary files;
- do not force the directory into a template shape;
- only install `SYSTEM.md` and `AGENTS.md`, then hand off to `SYSTEM.md`.

## Update Existing llmfs Protocol

Use when the target directory already has a trusted `SYSTEM.md` and the human asks to update llmfs itself.

This mode updates only bootstrap protocol entry points:

- `SYSTEM.md`
- `AGENTS.md`

It does not reorganize content, migrate indexes, rewrite logs, or change other filesystem structure. Those actions belong to the target directory's `SYSTEM.md` after handoff.

Use only templates packaged with this installed skill:

- `templates/SYSTEM.md`
- `templates/AGENTS.md`

Workflow:

1. Resolve the target path.
2. Read the target's current `SYSTEM.md`.
3. Read this skill package's `templates/SYSTEM.md` and `templates/AGENTS.md`.
4. Compare the current target `SYSTEM.md` with the packaged `templates/SYSTEM.md`.
5. Report the current target heading, packaged template heading, and whether the files differ.
6. If the target `SYSTEM.md` differs from the packaged template, do not replace it without explicit human approval.
7. If approved, replace or patch only `SYSTEM.md` using `templates/SYSTEM.md`.
8. Ensure `AGENTS.md` still routes to `SYSTEM.md` and does not override it; patch only the minimal router semantics when needed.
9. Stop using this skill as the operating authority.
10. Read the updated target `SYSTEM.md`.
11. Follow the updated target `SYSTEM.md` for logging, verification, and git commit policy.

Do not refer to development-only release archives or drafts outside the installed skill package during normal use. The installable package is self-contained: `SKILL.md` plus its `templates/` files.

## Check

Check only the bootstrap layer:

- `SYSTEM.md` exists;
- `AGENTS.md` exists;
- `AGENTS.md` routes agents to `SYSTEM.md`;
- `AGENTS.md` says `SYSTEM.md` wins on conflict;
- `SYSTEM.md` declares itself the local operating authority;
- if the target is not a git repository, recommend initializing git for local rollback checkpoints.

Do not treat this as a full filesystem health check. Full health checks belong to the target directory's `SYSTEM.md`.

## Safety Rules

1. Never overwrite existing `SYSTEM.md` without explicit human approval.
2. Never overwrite existing `AGENTS.md` blindly; patch only minimal router semantics when safe.
3. Never move existing files during adoption unless explicitly requested.
4. Never create hidden databases, vector stores, embedding caches, RAG indexes, `.llmfs/` state, or SQLite catalogs.
5. Never use agent skill memory as filesystem state.
6. Never treat this skill text as authority after bootstrap.
7. Always hand off authority to the target directory's `SYSTEM.md`.
8. During update-protocol, use only templates included in this installed skill package unless the human explicitly provides another source.
