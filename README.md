# llmfs

`llmfs` is a minimal bootstrap skill for installing local protocol entry points into a target directory.

The installable skill package lives in `llmfs/`.

Current structure:

```text
llmfs/
├── README.md
├── protocol/
│   └── SYSTEM.md
└── llmfs/
    ├── SKILL.md
    └── templates/
        └── AGENTS.md
```

## Workflow

- Edit and iterate the candidate protocol in `protocol/SYSTEM.md`.
- When the protocol is stable, promote it to `llmfs/templates/SYSTEM.md`.
- Install the skill by copying the inner `llmfs/` directory into an agent skill directory.

`llmfs` itself only installs `SYSTEM.md` and `AGENTS.md`. Any `README.md`, `INDEX.md`, `LOG.md`, `inbox/`, or other filesystem structure is created later by the target directory's `SYSTEM.md`.

No GitHub remote is configured yet. This repository is currently local-only.
