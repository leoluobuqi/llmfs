---
name: llmfs
description: Bootstrap a directory by installing SYSTEM.md and AGENTS.md. After bootstrap, the target SYSTEM.md is the local operating authority.
version: 0.1.0
metadata:
  hermes:
    tags: [filesystem, plaintext, bootstrap, protocol]
---

# llmfs

## Authority Boundary

This skill only installs local protocol entry points.

After bootstrap, the target directory's `SYSTEM.md` is the local operating authority.

Never use this skill to override, replace, or reinterpret an existing `SYSTEM.md` without explicit human approval.

If this skill conflicts with the target directory's `SYSTEM.md`, follow the target directory's `SYSTEM.md`.

## Purpose

Use this skill to bootstrap a target directory by installing:

```text
<target>/
├── SYSTEM.md
└── AGENTS.md
```

This skill does not create `README.md`, `INDEX.md`, `LOG.md`, `inbox/`, or any other filesystem structure. Those are created later by the target directory's `SYSTEM.md` after handoff.

## Package Layout

The installable skill directory is this `llmfs/` directory.

```text
llmfs/
├── SKILL.md
└── templates/
    ├── SYSTEM.md
    └── AGENTS.md
```

During protocol development, `templates/SYSTEM.md` may not exist yet. Edit the candidate protocol in the project root at `protocol/SYSTEM.md`. Promote it to `llmfs/templates/SYSTEM.md` only when stable.

## Preflight

Before writing to a target directory:

1. Resolve the target path.
2. Check whether the target already has `SYSTEM.md`.
3. If `SYSTEM.md` exists, stop and report already governed unless the human explicitly approves replacement or migration.
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
