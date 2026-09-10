# AGENTS.md

This file only routes agents to the local protocol.

## SYSTEM.md discovery safety

To locate the governing `SYSTEM.md`, inspect only the current directory and its direct ancestor chain.

- Never use recursive searches such as `find ..`, `find ~`, or root-wide searches.
- Do not scan sibling directories or other descendants of parent directories.
- Check exact candidate paths one level at a time and stop at the nearest applicable `SYSTEM.md`.
- If the current directory already contains `SYSTEM.md`, read it directly without performing a discovery search.

A safe ancestor-only discovery pattern is:

```bash
dir="$PWD"
while :; do
  if [ -f "$dir/SYSTEM.md" ]; then
    printf '%s\n' "$dir/SYSTEM.md"
    break
  fi
  [ "$dir" = "/" ] && break
  dir="$(dirname "$dir")"
done
```

Before creating, moving, deleting, renaming, or modifying files:

1. Confirm whether the current directory is under a `SYSTEM.md`-governed tree.
2. Confirm the governing `SYSTEM.md` has been read for this task and is still active in the current context.
3. If not, read it before acting.

Before answering questions about this filesystem or retrieving information from it:

1. Confirm whether the current directory is under a `SYSTEM.md`-governed tree.
2. Confirm the governing `SYSTEM.md` has been read for this task and is still active in the current context.
3. If not, read it before answering.
4. Use the governing `SYSTEM.md` retrieval rules.
5. When the user mentions a topic, project, path, file, area, prior artifact, or stored decision that may belong to this filesystem, treat it as a filesystem retrieval task unless the user clearly says otherwise.
6. Start from `INDEX.md` when routing is needed, then use filename/path search, full-text search, local indexes, and a small set of relevant files.
7. Answer with concrete visible paths when possible.

Follow the governing `SYSTEM.md`.

If this file and `SYSTEM.md` conflict, `SYSTEM.md` wins.
