# llmfs

**A protocol and bootstrap skill for an LLM-managed filesystem.**

llmfs helps an AI agent organize, retrieve, and maintain local files using rules and context stored alongside the files themselves. Its operating protocol, navigation, and audit trail live in visible, ordinary files that people and future agents can read.

The aim is to make a folder understandable across sessions: what belongs here, where to find it, how to change it safely, and which decisions should survive beyond a conversation.

## Why llmfs?

Over time, a working folder accumulates documents, drafts, code, references, and decisions. A chat may explain their relationships today, but a later session may lack that context. Naming and organizing everything by hand also becomes ongoing work.

llmfs gives the agent a persistent local protocol for that work. You describe what you need or put incoming material in `inbox/`; the agent uses visible files to organize content, maintain navigation, and record meaningful changes. The directory structure grows from actual content rather than a predefined taxonomy.

This can support personal research, writing, project materials, and other collections where files and their context need to remain useful over time. Content can include any file type, including PDFs, images, code, and databases. The protocol and organizational state remain readable without a hidden catalog, vector store, or chat history.

## How it works

The project has two layers:

- **The bootstrap skill** installs or checks the local protocol entry points. The installable package is the inner [`llmfs/`](llmfs/) directory.
- **The filesystem protocol** guides subsequent work in the target directory. After installation, the agent reads and follows that directory's `SYSTEM.md`.

A newly initialized directory starts with these entry points:

```text
<managed-root>/
├── SYSTEM.md
├── AGENTS.md
├── README.md
├── INDEX.md
├── LOG.md
└── inbox/
```

| Entry | Purpose |
| --- | --- |
| `SYSTEM.md` | The local operating protocol: organization, retrieval, safety, and maintenance rules. |
| `AGENTS.md` | Routes agents to the applicable `SYSTEM.md`. |
| `README.md` | Explains the directory's purpose, scope, and human use. |
| `INDEX.md` | Routes readers to relevant areas and important entry points. |
| `LOG.md` | Records meaningful operations in an append-only audit trail. |
| `inbox/` | Receives incoming files awaiting organization. |

The bootstrap skill installs only `SYSTEM.md` and `AGENTS.md`. The remaining structure is created afterward by the agent following the installed protocol. Existing files are preserved during initialization; adopting a folder does not automatically reorganize its contents.

During ordinary work, the agent starts retrieval from visible navigation, uses filename and text search, and reads relevant files. Reusable decisions and ongoing context belong in appropriate documents; `LOG.md` records operations and does not replace those documents. Human review and direct editing remain possible because the durable state is visible.

## Getting started

You need an AI agent that can read local instructions and inspect and edit files in the intended directory. Git is recommended for rollback checkpoints; it is optional for a managed directory.

### 1. Get the package

```sh
git clone https://github.com/leoluobuqi/llmfs.git
```

If your agent supports installing skills from a directory, copy the repository's inner `llmfs/` directory into the agent's documented skill location. Keep `SKILL.md` and `templates/` together. The package is self-contained; the development-only `protocol/` archive is not needed for installation.

Alternatively, explicitly ask a file-capable agent to read the checked-out [`llmfs/SKILL.md`](llmfs/SKILL.md) and apply it to a named target directory. Automatic skill discovery and support for `AGENTS.md` depend on the agent you use.

### 2. Initialize or adopt a directory

For a new directory, give the agent a concrete target path:

> Use the llmfs skill to initialize `<target-directory>` as an LLM-managed filesystem. After installing the protocol entry points, read its SYSTEM.md and complete initialization.

For an existing collection:

> Use the llmfs skill to adopt `<existing-directory>`. Preserve the existing files and layout, install the protocol entry points, and follow SYSTEM.md to complete setup.

Review the target before approving setup. The skill asks before initializing Git and requires explicit approval before replacing an existing `SYSTEM.md` or establishing a nested protocol boundary inside an already managed directory.

### 3. Use it in subsequent sessions

Work with the agent in the managed directory. Ensure it reads the applicable `SYSTEM.md` before retrieving information or changing files; if the agent does not load `AGENTS.md` automatically, explicitly ask it to read that entry point.

Examples of everyday requests:

- **Organize incoming material:** “Process the files in inbox, place them in appropriate areas, and update navigation and the operation log.”
- **Retrieve prior work:** “Find our notes about the launch plan and show the relevant file paths.”
- **Preserve a decision:** “Save the decision we just made in the project notes, with enough context for a future session.”
- **Maintain a collection:** “Review this area's navigation for stale links and suggest improvements before moving existing files.”

These are requests to the agent, not shell commands. Ordinary discussion is not saved automatically. File moves, deletion, replacement, and external sharing remain subject to the installed protocol and your authorization.

## Checking and updating

The skill supports four modes:

| Mode | Scope |
| --- | --- |
| `init` / `bootstrap` | Install protocol entry points for a new target, then hand off to its `SYSTEM.md`. |
| `adopt` | Install entry points in an existing directory while preserving its contents. |
| `check` | Inspect the bootstrap files and their routing; this is not a full filesystem health audit. |
| `update-protocol` | Compare the target's protocol with the packaged templates and, with explicit approval for replacement, update the entry points. |

Updating your installed skill package does not automatically update managed directories. Ask the agent to compare a specific directory against the package before approving a protocol update. Index maintenance and content reorganization are separate operations.

Current package: **0.2.7**. Current released protocol: **System Protocol v9 (2026-09-10)**. See the [protocol version index](protocol/INDEX.md) for release history.

## Boundaries and limitations

- **Agent compliance is required.** llmfs supplies written instructions, not a background service or enforcement engine. It does not guarantee that an agent will discover or correctly follow every rule.
- **A protocol is not a security boundary.** It cannot provide OS isolation, protect credentials from arbitrary processes, or replace filesystem permissions and other technical safeguards.
- **File access is not universal content understanding.** Reading PDFs, images, or other formats may require additional tools. Agents should distinguish inspected content from filename-based guesses.
- **Git supports recovery.** It does not replace visible documentation or provide an automatic off-machine backup. Under the current protocol, verified and logged file-changing tasks in a Git-managed root require a local task-level commit; pushing requires an explicit request.
- **Portability has limits.** Ordinary files make the state inspectable across tools, but operational compatibility across agents has not been established. Validate the workflow with your chosen agent.

## Repository and maintenance

```text
llmfs/
├── README.md
├── llmfs/                  # Installable bootstrap skill
│   ├── SKILL.md
│   └── templates/
│       ├── AGENTS.md
│       └── SYSTEM.md       # Current released install template
└── protocol/               # Development and release history
    ├── INDEX.md
    ├── SYSTEM.v0.md … SYSTEM.v9.md
    └── SYSTEM.next.md      # Mutable draft; not an install template
```

Future protocol changes begin in `protocol/SYSTEM.next.md`. Released `SYSTEM.vX.md` snapshots are immutable. Follow the review and release process in [protocol/INDEX.md](protocol/INDEX.md) before publishing a new version and updating the packaged template and version metadata.

Releasing the package and updating a managed directory are separate steps. A release does not authorize replacing any directory's installed `SYSTEM.md`.
