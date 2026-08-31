# For agents

METHOD.md wins if this page disagrees. Adapters (`AGENTS.md`,
`.hermes.md`) are generated. Do not hand-edit them.

## Boot or stop

`git pull --ff-only` (no origin → local mode). Read `STATE.md`.
Read METHOD §1. Then:

`python3 bin/boot`

Emit that line verbatim. No edits before it.

`BOOT ok · keel v… · <phase> · <n ready> · epics <ids or none> · <cue>`

No task and nothing ready → report and **wait**. Do not invent work.

## Route

- Story ID → that story + packed laws only.
- Epic ID → P-EPIC-RUN, this epic only.
- `sign` / `accept` / `park` / `reassign` / `epic-accept` only after
  the operator says so. Never self-sign. Never self-accept.

## Execute (one story)

```
python3 bin/claim US-xx
python3 bin/pack US-xx --save
# work inside the write-set
# evidence each AC and DoD (commit hash or artifact path)
python3 bin/review US-xx
python3 bin/state && python3 bin/render && python3 bin/verify
# commit review · stop. Accept or reject.
```

Write-set: `src/`, `archive/evidence/`, `archive/packs/`.
Status verbs: `bin/claim`, `bin/review`. Done is forbidden.
Do not write `METHOD.md` or `bin/` unless this story is about them.

## Forbidden

- park, reassign, epic-accept
- `git worktree add` as a KEEL verb (cwd is given)
- piggyback: a fix not in the packed ACs is a follow-up story
- unevidenced “done”

## Working set

The pack is the working set. Do not reload METHOD every file.
Last decisions are already in the pack. Overview is for the human.
