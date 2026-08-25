# stack

Versioned toolchain constraints. Packs bind execution to this file.

## Toolchain

- `bin/` is stdlib Python only.
- No pip installs.
- No build step.

## Artifacts

- Canonical store is markdown and JSON files in git.
- No external database.

## Runtime

- Git is the only runtime dependency.
- Git inspection commands run with `--no-pager` (or `GIT_PAGER=cat`); agent shells have no TTY.
