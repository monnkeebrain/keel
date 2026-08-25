# KEEL — METHOD

Operating manual for any agent (or human) working in this repo.
Adapters are generated from this file. Do not edit adapters directly.

Status: SKELETON — fill during P0. Keep every section minimal enough
to be unambiguous; ambiguity here becomes thrash in every future session.

## 0. Reading order for a fresh session

1. Read `STATE.md` (bounded snapshot + resume cue)
2. Read the law slices relevant to your task (see pack or ask operator)
3. Read the target story in `data/stories.json`
4. Work. Then: update STATE.md, append residue to `decisions.md`, commit.

Bootstrap ritual for ephemeral environments (Open Terminal containers etc.):
`git pull && cat STATE.md` before anything else.

## 1. Invariants (Tier A — change rarely, see AMENDMENTS.md)

1. Everything that matters lives in this repo, never only in a harness.
2. STATE.md stays bounded: O(current), never O(history).
3. No done without evidence mapped to AC/DoD.
4. One canonical store (`data/`, `laws/`, `decisions.md`);
   pages/views are projections.
5. Raw history goes to `archive/`; durable lessons get distilled into
   `decisions.md`.
6. IDs (story ids, law keys) are immutable once referenced anywhere.
7. Adapters are build artifacts; METHOD.md is their only source.
8. Complexity must pay rent.

## 2. Artifacts

| Path | Role | Mutability |
|---|---|---|
| `laws/*.md` | standing constraints: stack, design, domain rules | versioned, append new versions |
| `data/stories.json` | contracts: narrative, AC[], DoD[], status | append/edit pre-ready; locked at launch |
| `decisions.md` | residue: rationale + consequences, newest first | append-only; corrections via supersede |
| `STATE.md` | live snapshot: phase, open intents, next step | regenerated freely |
| `archive/log-*.md` | raw history | write-once |

Story schema v0 = the validated 12-story corpus in `data/stories.json`
(id, epic, priority, title, as_a, i_want, so_that, ac[], dod[], status).
Do not extend fields without an AMENDMENTS entry.

## 3. Protocols (Tier B — amendable)

### P-BOOT Session bootstrap
TODO: exact steps + forbidden actions before STATE.md is read.

### P-STORY Contract flow
capture intent → draft story entry → `bin/gate check` passes →
operator signs (commit message: `sign US-xx`). No self-promotion to
ready without the sign commit.

### P-RUN Execution flow
`bin/pack compile US-xx` → execute within scope → verify each AC/DoD →
report evidence list → operator accepts/rejects → status update +
decision entry if material → commit `accept US-xx`.

### P-DISTILL Compaction ritual
At phase end or when archive grows: run distill, move raw log to
`archive/`, keep decisions.md tight. STATE.md regenerated after.

## 4. Toolchain

`bin/gate` · `bin/pack` · `bin/state` · `bin/distill` — stdlib Python,
no dependencies. Built in P1. Until then, protocols run manually.

## 5. Harness notes

- Hermes Agent: reads AGENTS.md/.hermes.md natively; skills via
  agentskills.io format; terminal tool runs bin/* directly.
- Open WebUI + Open Terminal: system-prompt adapter points here;
  containers may be ephemeral — bootstrap ritual is mandatory there.
- Codex / OpenCode: read AGENTS.md natively.
