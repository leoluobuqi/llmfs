# llmfs

`llmfs` is a minimal bootstrap skill for installing local protocol entry points into a target directory.

The installable skill package lives in `llmfs/`.

Current structure:

```text
llmfs/
├── README.md
├── protocol/
│   ├── INDEX.md
│   ├── SYSTEM.v0.md
│   ├── SYSTEM.v1.md
│   ├── SYSTEM.v2.md
│   ├── SYSTEM.v3.md
│   ├── SYSTEM.v4.md
│   ├── SYSTEM.v5.md
│   ├── SYSTEM.v6.md
│   ├── SYSTEM.v7.md
│   ├── SYSTEM.v8.md
│   └── SYSTEM.next.md
└── llmfs/
    ├── SKILL.md
    └── templates/
        ├── AGENTS.md
        └── SYSTEM.md
```

## Workflow

待发布：`llmfs/templates/AGENTS.md` 已同步祖先链限定的 `SYSTEM.md` 查找声明及示例；package 版本将在下一次发布时统一更新，冻结的 `SYSTEM.next.md` 正文未变。

- Edit and iterate the next candidate protocol in `protocol/SYSTEM.next.md`.
- Released protocol snapshots live in immutable `protocol/SYSTEM.vX.md` files.
- When the protocol is stable, release it by copying `protocol/SYSTEM.next.md` to both `protocol/SYSTEM.vX.md` and `llmfs/templates/SYSTEM.md`.
- Install the skill by copying the inner `llmfs/` directory into an agent skill directory.

`llmfs` itself only installs `SYSTEM.md` and `AGENTS.md`. Any `README.md`, `INDEX.md`, `LOG.md`, `inbox/`, or other filesystem structure is created later by the target directory's `SYSTEM.md`.

No GitHub remote is configured yet. This repository is currently local-only.
