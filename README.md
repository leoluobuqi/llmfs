# llmfs

[English](README.md) | [简体中文](README.zh-CN.md)

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
├── LICENSE.llmfs
├── README.md
├── INDEX.md
├── LOG.md
└── inbox/
```

| Entry | Purpose |
| --- | --- |
| `SYSTEM.md` | The local operating protocol: organization, retrieval, safety, and maintenance rules. |
| `AGENTS.md` | Routes agents to the applicable `SYSTEM.md`. |
| `LICENSE.llmfs` | Preserves the llmfs license separately from the agent instructions. |
| `README.md` | Explains the directory's purpose, scope, and human use. |
| `INDEX.md` | Routes readers to relevant areas and important entry points. |
| `LOG.md` | Records meaningful operations in an append-only audit trail. |
| `inbox/` | Receives incoming files awaiting organization. |

The bootstrap skill installs `SYSTEM.md`, `AGENTS.md`, and their separate `LICENSE.llmfs` notice. The remaining structure is created afterward by the agent following the installed protocol. Existing files are preserved during initialization; adopting a folder does not automatically reorganize its contents.

During ordinary work, the agent starts retrieval from visible navigation, uses filename and text search, and reads relevant files. Reusable decisions and ongoing context belong in appropriate documents; `LOG.md` records operations and does not replace those documents. Human review and direct editing remain possible because the durable state is visible.

## Getting started

Start with an AI agent that can retrieve files from GitHub, read local instructions, and inspect and edit files in your chosen directory. Git is recommended for rollback checkpoints; it is optional for a managed directory.

### 1. Open your target directory

Open the directory you want to manage in your agent, or give it the exact target path. You can start with an empty directory or an existing collection of files.

### 2. Ask the agent to initialize it

Paste this request into the agent. If you have not opened the target directory, replace “the current directory” with its exact path:

> Use the llmfs skill at https://github.com/leoluobuqi/llmfs/tree/main/llmfs to initialize the current directory. Preserve existing content and ask before replacing protocol files. Follow the installed SYSTEM.md to complete setup.

Review the target before approving setup. The skill asks before initializing Git and requires explicit approval before replacing an existing `SYSTEM.md` or establishing a nested protocol boundary inside an already managed directory.

The agent only needs the bootstrap package to perform setup. This request does not require a permanent skill installation or a full development checkout. Remote retrieval and local file operations depend on the agent's tools and permissions; this workflow has not been validated across all agents. If remote retrieval is unavailable, provide the complete `llmfs/` package locally, including `SKILL.md`, `LICENSE`, and `templates/`, and ask the agent to read the local `SKILL.md`.

### 3. Use it in subsequent sessions

Work with the agent in the managed directory. Ensure it reads the applicable `SYSTEM.md` before retrieving information or changing files; if the agent does not load `AGENTS.md` automatically, explicitly ask it to read that entry point.

Examples of everyday requests:

- **Organize incoming material:** “Process the files in inbox, place them in appropriate areas, and update navigation and the operation log.”
- **Retrieve prior work:** “Find our notes about the launch plan and show the relevant file paths.”
- **Preserve a decision:** “Save the decision we just made in the project notes, with enough context for a future session.”
- **Maintain a collection:** “Review this area's navigation for stale links and suggest improvements before moving existing files.”

These are requests to the agent, not shell commands. Ordinary discussion is not saved automatically. File moves, deletion, replacement, and external sharing remain subject to the installed protocol and your authorization.

### Optional: install the skill for repeated use

If you regularly initialize or update managed directories, install the repository's inner [`llmfs/`](llmfs/) directory using your agent's supported skill installation method. Keep [`SKILL.md`](llmfs/SKILL.md), [`LICENSE`](llmfs/LICENSE), and `templates/` together. The package is self-contained; the development-only `protocol/` archive is not needed. Installation paths and automatic skill discovery depend on the agent you use.

### Hermes Agent: avoid overlapping llm-wiki workflows

When using Hermes Agent to manage an llmfs directory, we recommend disabling the bundled `llm-wiki` skill for that workflow to avoid applying two sets of file-organization and knowledge-maintenance rules to the same directory. The local `SYSTEM.md` should govern organization, navigation, and maintenance.

Use the skill-management controls supported by your Hermes version to disable only `llm-wiki`; do not disable the entire skills system or delete existing content. If you need `llm-wiki` for other work, keep that workflow separate, using a dedicated profile where supported. Start a new session after the change and verify that `llm-wiki` is no longer loaded for llmfs tasks. This is a workflow precaution, not a claim that the two tools always conflict or that disabling a skill provides a security boundary.

### Optional: Hermes Agent skips local commits

If Hermes has read the applicable `SYSTEM.md` but still skips a required local commit because it expects another explicit request, clarify the existing authorization. If it has not read the protocol, fix that first; this setting is not a substitute for loading project rules or resolving a failed Git command.

Use Hermes' [configuration tools](https://hermes-agent.nousresearch.com/docs/user-guide/configuration) to append the following sentence to `agent.coding_instructions` in the profile you use, preserving any existing instructions:

> If the user-approved rules for the current repository explicitly require local commits, treat that requirement as existing authorization; do not ask for confirmation again.

The usual configuration locations are:

- Default profile: `~/.hermes/config.yaml`.
- Named profile: `~/.hermes/profiles/<name>/config.yaml`.

[Profiles have independent configuration](https://hermes-agent.nousresearch.com/docs/user-guide/profiles). Apply the clarification only to profiles that need it; changing the default profile does not update other profiles. Keep the workflow rules in `SYSTEM.md` rather than duplicating them here.

This supplements Hermes' coding guidance; it does not enforce commits. Start a new session and verify that an ordinary file-changing task ends with the required local commit without an additional reminder. This is an optional workaround, not a guarantee of compliance or a required llmfs setup step.

## Checking and updating

The skill supports four modes:

| Mode | Scope |
| --- | --- |
| `init` / `bootstrap` | Install protocol entry points for a new target, then hand off to its `SYSTEM.md`. |
| `adopt` | Install entry points in an existing directory while preserving its contents. |
| `check` | Inspect the bootstrap files and their routing; this is not a full filesystem health audit. |
| `update-protocol` | Compare the target's protocol with the packaged templates and, with explicit approval for replacement, update the entry points. |

Updating your installed skill package does not automatically update managed directories. Index maintenance, link conversion, and content reorganization are separate operations.

### Ask the agent to update an existing directory

Open the managed directory and paste this request. If it is not the current directory, replace “the current directory” with its exact path:

> Use the llmfs skill at https://github.com/leoluobuqi/llmfs/tree/main/llmfs in update-protocol mode for the current directory. First read the installed SYSTEM.md, compare it with the packaged template, and report the versions, differences, and any local customizations. Ask for my approval before making changes. After approval, update the protocol entry points and license notice as needed, preserve unrelated content, and follow the updated SYSTEM.md for verification, logging, and local commits.

Review the proposed changes before approving; local customizations should not be silently discarded. This updates `SYSTEM.md`, related `AGENTS.md` routing, and `LICENSE.llmfs` as needed—not the directory's content or existing links. If remote retrieval is unavailable, provide the complete updated `llmfs/` package locally and use its `SKILL.md` instead.

Current package: **0.2.10**. Current released protocol: **System Protocol v10 (2026-10-31)**. See the [protocol version index](protocol/INDEX.md) for release history.

## Boundaries and limitations

- **Agent compliance is required.** llmfs supplies written instructions, not a background service or enforcement engine. It does not guarantee that an agent will discover or correctly follow every rule.
- **A protocol is not a security boundary.** It cannot provide OS isolation, protect credentials from arbitrary processes, or replace filesystem permissions and other technical safeguards.
- **File access is not universal content understanding.** Reading PDFs, images, or other formats may require additional tools. Agents should distinguish inspected content from filename-based guesses.
- **Git supports recovery.** It does not replace visible documentation or provide an automatic off-machine backup. Under the current protocol, verified and logged file-changing tasks in a Git-managed root require a local task-level commit; pushing requires an explicit request.
- **Portability has limits.** Ordinary files make the state inspectable across tools, but operational compatibility across agents has not been established. Validate the workflow with your chosen agent.

## Repository and maintenance

To work on llmfs itself, clone the development repository:

```sh
git clone https://github.com/leoluobuqi/llmfs.git
```

```text
llmfs/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── llmfs/                  # Installable bootstrap skill
│   ├── SKILL.md
│   ├── LICENSE
│   └── templates/
│       ├── AGENTS.md
│       └── SYSTEM.md       # Current released install template
└── protocol/               # Development and release history
    ├── INDEX.md
    ├── SYSTEM.v0.md … SYSTEM.v10.md
    └── SYSTEM.next.md      # Mutable draft; not an install template
```

Future protocol changes begin in `protocol/SYSTEM.next.md`. Released `SYSTEM.vX.md` snapshots are immutable. Follow the review and release process in [protocol/INDEX.md](protocol/INDEX.md) before publishing a new version and updating the packaged template and version metadata.

Releasing the package and updating a managed directory are separate steps. A release does not authorize replacing any directory's installed `SYSTEM.md`.

## License

llmfs is licensed under the [MIT License](LICENSE), covering the project's skill, protocol templates, release snapshots, and documentation. Copyright (c) 2026 Leo Luo.

The installable package includes its own identical [LICENSE](llmfs/LICENSE). Keep the copyright and license notice with copies or substantial portions of llmfs, including templates distributed separately. During setup, the skill preserves the complete notice in a separate `LICENSE.llmfs`, identifying its scope as the llmfs-derived portions of `AGENTS.md` and `SYSTEM.md`. The notice is not embedded in agent instructions or required reading during ordinary work.

Using llmfs does not change the ownership or licensing of the documents, code, or other content you manage with it.
