# KEEL

**Build & ship. The keel holds.**

A super-thin method for humans and AI agents, dropped into a git
repo: signed user stories, standing laws, a live snapshot, and a
CLI any agent can run. Not another coding assistant. Not a harness.
The **project** is the memory. Swap Claude for Grok tomorrow — the
work does not move.

![Hermes session: BOOT line, AC table, agent refusing to piggyback a
fix outside packed scope](screenshots/keel-proof-hermes.png)

The agent printed `BOOT ok`, mapped every acceptance criterion, and
**stopped** when a packed `scope.forbid` blocked the only way to
satisfy an AC. It asked to expand scope instead of improvising.
That is the product.

**Disclaimer:** Alpha. Use at your own risk.

---

## Why this exists

Chat is a terrible place to keep a project. You brief an agent. It
writes code. The thread dies, the model changes, the tool gets
deprecated — and the context that made the work coherent is gone.

KEEL puts that context in ordinary files. A fresh chat with zero
history can boot, report status, and continue. You still hold the
accept key.

## What makes it special

1. **The story is the contract.** Narrative + AC[] + DoD[]. If you
   cannot say how you would verify it tomorrow morning, it does not
   get signed. `bin/gate` is a linter for intent. Excellent stories
   are the whole game; the CLI only makes them deterministic.
2. **The CLI does not improvise.** `bin/claim`, `bin/pack`,
   `bin/review`, `bin/verify` — stdlib Python, no pip, no build.
   Same commands in Hermes, Claude Code, Codex, Open WebUI, or a
   VPS. Prompts drift. These do not.
3. **Humans accept. Agents stop.** A “done” claim without a commit
   hash or an artifact path is void. The gate held on every model
   we tested — including when nobody reminded the agent.

Provenance: `docs/testing-matrix.md`. Harnesses and models listed
there only.

**Status:** v0.1.2. Method and CLI are in working condition. Epic
dispatch (`take E-n`) and `bin/review` are in METHOD. One unsigned
workshop story remains (`US-18`). Clone, `bin/init`, ship.

---

## Try it

```bash
git clone https://github.com/monnkeebrain/keel.git my-project
cd my-project
bin/init
git add -A && git commit -m "init workspace"
```

Open the folder in the agent you already use. Prefer a click? GitHub
**Use this template**, or unzip and `bin/init` (it creates `.git` if
missing). Local-only is first-class. Sync is `git push` / `git pull`.

New here? [Guide for humans](docs/for-humans.md).
Agent in the repo? [Guide for agents](docs/for-agents.md) — then `boot`.

---

## The loop

1. **Boot** — agent emits `BOOT ok · …` before touching a file.
2. **Sign** — you turn intent into a gated story (`bin/gate`, then
   `sign US-xx`). Unverifiable work is parked, not shipped.
3. **Take** — `take US-xx` or `take E-n`. One pack at a time.
   `bin/claim` → `bin/pack` → work → evidence → `bin/review`.
4. **Accept** — you judge on `overview.html`. Agents never accept.
5. **End** — `bin/state`, `bin/render`, `bin/verify`, commit, push.

---

## Where things live

| Path | Role |
|---|---|
| `METHOD.md` | operating manual — only hand-written instruction source |
| `data/stories.json` | contracts (narrative, AC[], DoD[], status) |
| `data/epics/` | epic envelopes |
| `laws/` | standing constraints (stack, design, domain) |
| `decisions.md` | residue, newest first |
| `STATE.md` | bounded snapshot (regenerated) |
| `overview.html` | your decision surface (`bin/render`) |
| `archive/` | packs and evidence, write-once |
| `src/` | your project's code |
| `bin/` | dumb CLI |
| `AGENTS.md` / `.hermes.md` | generated adapters |

---

## More

[For humans](docs/for-humans.md) · [For agents](docs/for-agents.md) ·
[Tips](docs/tips-and-tricks.md) · [Testing matrix](docs/testing-matrix.md) ·
[System](docs/system.md)

Failures → `FAILURES.md`. Method changes → `AMENDMENTS.md`.
Invariants: METHOD.md §1. Complexity must pay rent.

---

## Changelog

v0.1.2
- US-31 METHOD P-EPIC protocols (text only)
- US-32 One file per epic plus epic-gate and epic-claim
- US-33 take E-n chains unlocked stories and presents the epic
- US-34 Optional story depends_on; pack and claim refuse if predecessor is not done
- US-35 Operator accept E-n, park, and reassign
- US-36 Overview groups stories by epic
- US-37 VERSION 0.1.2, README current, BOOT line includes epics
- US-38 cwd is given; agent does not create worktrees
- US-39 bin/review flips in_progress to review
- Human and agent guides; README proof screenshot

v0.1.1
- US-19 VERSION file is the single version string
- US-20 bin/claim sets status to in_progress
- US-21 Pack names the default write-set
- US-22 Optional scope.touch and scope.forbid on stories
- US-23 Overview shows image evidence for review stories
- US-24 Matrix and README list every tested harness and model
- US-25 One writer per working copy; worktrees named as later isolation
- F-007 bin/state KeyError on missing blocked bucket · D-014
