# E8 — Isolation (final plan, not METHOD yet)

Status: final candidate. Not in force until signed E8 stories land
in METHOD.md and `bin/`. Tier B. Invariants do not move.

Abandoned US-27..30 stay abandoned. E8 gets new ids.

This plan is written from dogfood (building KEEL with KEEL), not
from a story-grain worktree sketch. Epic is the dispatch unit.
Worktrees are the isolation primitive. `bin/wt` is the CLI.

---

## 1. Why this exists

E7 made `take E-n` real for **1 human × 1 agent × 1 working copy**.
Serial model-swap already works. Two agents both rewrite
`data/stories.json`, `STATE.md`, and `overview.html`. ff-only
rejects the second push. That is the bug.

Delegating **stories** to agents is legal (`take US-xx` stays).
Delegating **epics** is the product. One agent, one epic, one
isolation checkout. Stories chain inside it.

---

## 2. Load-bearing rule

> Isolation checkouts multiply editors of `src/` and `archive/`.
> They do not multiply writers of the canonical store.

**Main** (the checkout the operator reads) is the only writer of:
`data/`, `STATE.md`, `decisions.md`, `FAILURES.md`, `laws/`,
`METHOD.md`, `overview.html`, `AMENDMENTS.md`, adapters.

**Agent isolation checkout** may write:
`src/**`, `archive/packs/<story-id>-*`, `archive/evidence/<story-id>-*`.

`bin/wt accept` **imports those prefixes only**. It never merges
the agent branch as a whole. A forgotten verify must not be able
to land a store edit.

1:1 on main is unchanged. That checkout *is* the store writer.
Creating a worktree is **not** required for 1:1.

Story-per-file, `claimer.*`, and `bin/review` do **not** pay rent
under this rule. Deferred.

---

## 3. Dogfood that this plan is answering

- Agents piggyback (F-007). Write-scope is a `bin/verify` FAIL on
  the agent branch, not a sentence in a pack.
- Agents hand-edit the store whenever they can. So they must not
  be able to *land* those edits. Local scratch status in the
  worktree is allowed so chaining/`depends_on` still run; join
  discards `data/`.
- Per-story accept caught real defects. Keep per-story evidence.
  Do not merge AC lists. Batch *judgment* of an epic after
  present.
- Operator-only park/reassign/accept held when adapters forbade
  the commands. Same for `bin/wt add|accept|reject`.
- `take E-n` chaining is the tax cut. Isolation unit = epic, not
  story. Per-story worktrees would undo E7.
- Unevidenced done is void. Artifact path stays the citation.
  Do not squash. Do not rebase agent commits at accept.

---

## 4. Git rules (split, on purpose)

**Pull of `main`:** `git pull --ff-only`. Never merge. Agents,
clones, main — everyone. This rule does **not** change.

**Join at accept:** not a fast-forward of `main` to `agent/E-n`
(the second epic is never a descendant). Not a whole-branch
merge (that is how `data/` sneaks in).

`bin/wt accept E-n`:

1. List files that differ between `main` and `agent/E-n`.
2. FAIL if any path is outside the allowed prefixes.
3. FAIL if those paths conflict with `main` (planning bug:
   touch overlap escaped `epic-gate`).
4. Import allowed paths onto `main`, commit
   `accept E-n · import src+archive`.
5. Run today's epic-accept rules on the **main** store
   (review/done only; refuse in_progress/ready/backlog/blocked
   leftovers).
6. `bin/state` && `bin/render`.
7. `git worktree remove` (not hand-delete).

Linear `main`. Agent hashes remain on `agent/E-n` until the
operator deletes the branch. Artifact paths on main are the
evidence that matters.

---

## 5. The loop

**Dispatch (operator, main):** epic signed, `bin/wt add E-n`.
Creates worktree + branch `agent/E-n`, sets epic `in_progress`,
prints path + BOOT cue. Refuses if in-flight epics (BOOT active:
`in_progress`+`review`) would exceed 3. Refuses if this checkout
is already a linked worktree.

**Run (agent, isolation checkout):** P-BOOT, `git pull --ff-only`
of main (if it fails, stop). `bin/epic-pack` / `bin/pack` (read
the store; may scratch-claim locally). Write `src/` + archive
prefixes. One story at a time. Evidence per story. Do not run
`wt add|accept|reject`, park, reassign, epic-accept. Do not
commit store files expecting them to land.

**Present:** no unlocked+logical story left. Agent commits,
pushes `agent/E-n`, prints `PRESENT E-n`, stops. Does not write
the main store.

**Judge (operator, main):** `bin/wt present E-n` sets epic
`review` from the branch evidence index. Overview Needs-you.
`bin/wt accept E-n` or partial: park/reassign leftovers on
main, then accept. Partial is **status on main**. The import
is the whole presented `src/`+archive. If parked work is mixed
into `src/`, **reject, do not import.**

**Hotfix:** `take US-xx` on main. No worktree.

---

## 6. CLI (thin)

One dispatcher, stdlib, same wrapper pattern as `epic-claim`.

| Command | Who | Effect |
|---|---|---|
| `bin/wt add E-n` | operator | worktree + `agent/E-n` + epic in_progress; WIP 3 |
| `bin/wt ls` | anyone | id, path, branch, epic status |
| `bin/wt present E-n` | operator | epic → review from evidence index; no import |
| `bin/wt accept E-n` | operator | path-import + epic-accept + state/render + remove |
| `bin/wt reject E-n` | operator | no import; park/reassign as named; keep branch unless `--drop` |

`bin/verify`: `agent/*` vs main, allowed prefixes only (informational
on the branch; **blocking** inside `wt accept` step 2).

Adapters: agent must not run `wt add`, `wt accept`, `wt reject`.

Default path: sibling `../<repo>-E-n`. Override `--path`. Print
the path; the harness must open **that** folder. Refuse `wt add`
if `.git` is missing.

Existing `epic-claim` / `epic-present` / `epic-accept` remain the
**1:1 on main** path. Do not delete them. Do not make 1:1 call `wt`.

---

## 7. Flaws that remain (do not pretend)

**F1. Two loops.** 1:1 on main vs N on worktrees. Two P-END
commit sets. That is real complexity. It pays rent: 1:1 is the
common path and must not grow a worktree ritual. Document both.
Adapters: do not `wt add` unless the operator said so.

**F2. Scratch store vs landed store.** Chaining and `depends_on`
need status flips during the run. Those flips happen in the
worktree copy and **die at join**. Main never saw `review` until
`wt present`. If the agent "accepts" themselves in the scratch
copy, it does not land. Main is truth. Still teach "do not mark
done" — verify cannot catch a scratch `done`.

**F3. Partial accept cannot un-mix `src/`.** If the agent committed
parked work, reject the epic. There is no honest cherry-pick
protocol. Scope.touch per story inside an epic is the mitigation,
not a guarantee.

**F4. Sibling worktree path vs GUI harnesses.** Hermes/project
sidebars follow one folder. `../repo-E7` may be invisible until
the operator opens it. `--path` exists because of this. Not
solved by putting worktrees inside the repo (git add hazards).

**F5. Conflict detection at import.** If `epic-gate` missed an
overlap, import must FAIL, not overwrite. That check has to be
real (merge-tree or equivalent), not a comment.

**F6. Clones.** Worktrees are same-machine. Two machines = clone
on `agent/E-n`, same write-scope, no `git worktree`. `bin/wt add`
is worktree sugar only. Do not claim worktrees cover remote
agents.

**F7. WIP 3 is a human budget, not a scheduler.** Raising it
before the throwaway two-agent arm is green would be the same
class of miss as freezing a 1:1 skill before E7.

**F8. `wt accept` blast radius.** Import + status + render +
remove in one command is convenient and hard to undo. Sequence
must stop before status if import fails. Removing the worktree
last. No `--force` delete of operator data.

---

## 8. Out of E8

- Story-per-file, claimer, `bin/review`
- E9 official skill (after this is green)
- US-18 workshop
- Confirming A-004/A-005/A-006
- MCP, orchestrator, message bus

---

## 9. Dogfood gate

1. Land METHOD write-scope + `bin/wt` on this repo. 1:1 still
   works with zero worktrees (regression).
2. Throwaway `bin/init`: two epics, disjoint touch, two
   worktrees, both PRESENT, operator `wt accept` E-a then E-b.
   Second import must succeed with both `src/` trees present.
   Forbidden-path commit on an agent branch must make accept
   FAIL.
3. Until that arm is green, WIP stays 1.

---

## 10. Stories (draft only after you accept this plan)

1. METHOD: write-scope, split git rule, epic isolation unit,
   1:1 creates no worktree. Adapters forbid `wt *` mutating
   verbs. No `bin/wt` yet.
2. `bin/wt add|ls` + WIP 3 on add + default sibling path +
   `--path` + refuse if not main checkout / no `.git`.
3. `bin/verify` + `wt accept` path-import (allowed prefixes,
   conflict FAIL, no whole-branch merge) + `wt present` +
   `wt reject`. Render In-flight can read `wt ls`.
4. Throwaway two-agent arm documented as a resume-suite item
   (may be operator-attested, like extra matrix rows).

---

Accept this plan or name the flaw that still blocks it.
Do not sign E8 stories until then.
