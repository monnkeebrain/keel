# E8 — Isolation (candidate, not METHOD yet)

Status: plan, rewritten around the worktree review. Not in force.
No `bin/wt`, no merge-at-accept, no write-scope verify until signed
E8 stories land in METHOD.md and `bin/`.

Tier B. Invariants do not move. Abandoned US-27..30 stay abandoned.
E8 gets **new** story ids.

E7 is accepted. 1H × 1A × 1 working copy works. Serial model-swap
works. Parallel agents do not.

---

## 0. The load-bearing rule

> **Worktrees (or clones) multiply the places where `src/` and
> `archive/` get edited. They do not multiply the places where the
> canonical store gets written.**

Canonical store, main worktree only: `data/`, `STATE.md`,
`decisions.md`, `FAILURES.md`, `laws/`, `METHOD.md`, `overview.html`,
`AMENDMENTS.md`, adapters.

Agent checkouts may write: `src/**`, `archive/packs/US-xx-*`,
`archive/evidence/US-xx-*`.

1:1 on main is unchanged: that checkout *is* the store writer.

This rule makes method-file merge conflicts structurally impossible.
`bin/verify` on an `agent/*` branch: `git diff main --name-only`
must be inside the allowed prefixes, or FAIL. Evidence, not vibe.

Story-per-file **does not pay rent** under this rule (only main
writes `data/stories.json`). Deferred. Same for `claimer.*` and
`bin/review`.

---

## 1. What the external review got right

- Worktrees are the right primitive: shared object DB, cheap,
  disposable, same laws/history, isolated index/HEAD.
- Human stays accept key **and** dispatch key. Creating the
  isolation checkout is the claim.
- Thin `bin/wt` (add / ls / accept / reject), not an orchestrator.
- Artifact-path evidence is the stable citation (already KEEL).
- Pack already inlines laws — that *is* the pin for mid-flight
  law bumps. Grandfather. Do not rebase the agent.
- Cleanup through `bin/wt`, not hand-deleted folders.
- Needs a real `.git` (ZIP path: `bin/init` already creates one).

---

## 2. Logical mistakes (and the KEEL fix)

**Mistake 1 — one worktree per story kills E7.**
Chaining is the point of `take E-n`. Per-story worktrees restore
the dispatch tax. **Unit of isolation is the epic.** One agent,
one branch `agent/E-n`, one worktree (or clone). Stories chain
inside it. `take US-xx` stays legal as a hotfix on main.

**Mistake 2 — `git pull --ff-only` cannot land the second epic.**
Two agents branch from `main` at M. Operator fast-forwards A.
B is not a descendant of new main. ff-only **always fails** for
N>1. `--no-ff` merge is required at join, or a rebase that
rewrites hashes.
**Fix:** split the git rule. Agents and every clone **pull main
ff-only, never merge.** Operator join of `agent/E-n` into main
is an **operator-only merge** (`bin/wt accept`). Do not squash.
Do not rebase at accept. Agent commit hashes stay on the branch.
Prefer artifact-path evidence; record the merge commit in the
accept residue.

**Mistake 3 — skipping `review`.**
"in_progress at wt create, done at merge" drops Needs-you.
**Fix:** agent stops when no unlocked+logical story remains,
pushes `agent/E-n`, prints `PRESENT E-n`. Does not write the
store. Operator on main runs `bin/wt present E-n` (or `bin/wt
ls` + render) which sets epic `review` from the branch's
evidence index. Then judge. `bin/wt accept` is still operator.

**Mistake 4 — partial accept vs one branch.**
All epic src/ lives on `agent/E-n`. Cherry-picking accepted
stories is not a method. **Fix:** the branch contains only work
the agent is presenting. Parked/unlogical stories were not
committed. Partial = operator park/reassign leftover **ids**
on main, merge the branch (presented code only). If the agent
mixed parked work into src/, reject the epic, do not merge.

**Mistake 5 — "src/ overlap is a normal merge conflict."**
KEEL already forbids overlapping `scope.touch` on two non-done
epics (`epic-gate`). Overlap at merge is a **planning bug**.
Park/reassign; do not "just resolve."

**Mistake 6 — worktrees-only.**
Worktrees are same-machine. Two laptops = clones. **Same
write-scope contract.** `bin/wt add` is sugar for worktrees;
a clone on `agent/E-n` is the other legal isolation. 1:1 still
creates neither.

**Mistake 7 — `--no-ff` to save evidence hashes.**
Hashes survive a true merge without squash. Artifact paths
already survive everything. Don't squash. Don't rebase at
accept. `--no-ff` vs `--ff` is secondary; the second epic
cannot ff anyway.

**Mistake 8 — `bin/wt accept` marks done in one blast.**
Merge can fail. **Fix:** refuse status change if merge fails.
Sequence: merge → (fail: stop) → epic-accept semantics →
state/render → `git worktree remove`. Operator-only. Adapters
forbid agents from `wt accept` / `wt reject` / `wt add`.

**Mistake 9 — archive layout change.**
`archive/evidence/US-xx-*` and `archive/packs/US-xx-*` are
already unique. Do not invent `archive/US-xx/`.

**Mistake 10 — BOOT identity as `wt-US-14`.**
CLAIM already prints root + HEAD. BOOT already lists active
epics. Don't fork the BOOT schema. Optional: print `root`
on every boot (cheap). Attribution = branch name `agent/E-n`.

---

## 3. Mapping onto the five beats (epic grain)

1. **P-BOOT** — agent opens the isolation checkout, pulls
   ff-only, emits the existing BOOT line. Root is in git.
2. **P-STORY / P-EPIC-SIGN** — main only. Operator signs
   stories and the epic, then dispatches.
3. **P-EPIC-RUN** — operator `bin/wt add E-n` (worktree +
   branch `agent/E-n` + epic `in_progress`). Agent packs
   (read-only), writes `src/` + their archive prefixes,
   commits on `agent/E-n`. Does **not** run claim/review/
   state/render/epic-present/epic-accept. Chains unlocked
   logical stories by packing the next one; evidence files
   are the per-story grain.
4. **P-EPIC-PRESENT** — agent pushes, prints PRESENT, stops.
   Operator on main projects review (evidence index on the
   branch) onto the store.
5. **P-EPIC-ACCEPT / P-END** — operator `bin/wt accept E-n`
   or partial park/reassign then accept. Merge, then store,
   then state/render, then remove worktree. Decisions land
   on main.

`take US-xx` on main (hotfix) does not create a worktree.

---

## 4. Thin CLI

One new dispatcher, stdlib, wrappers if needed:

| Command | Who | Effect |
|---|---|---|
| `bin/wt add E-n` | **operator** | `git worktree add -b agent/E-n ../wt-E-n main`; epic → in_progress; refuse if WIP would exceed 3; print root + BOOT cue |
| `bin/wt ls` | anyone | worktrees/branches with epic id + status; render In-flight reads this |
| `bin/wt present E-n` | operator (or a read of the branch) | epic → review if evidence index exists; does not merge |
| `bin/wt accept E-n` | **operator** | merge `agent/E-n` into main (no squash); then today's epic-accept rules; state; render; `worktree remove` |
| `bin/wt reject E-n` | **operator** | keep or drop branch; park/reassign as operator says; do not merge |

`bin/verify`: on `agent/*`, allowed path prefixes only.

Existing `epic-claim` / `epic-present` / `epic-accept` stay for
**1:1 on main**. `bin/wt *` is the N-agent path. Do not make
1:1 create a worktree.

WIP 3 lives on `bin/wt add`, not on story claim. In-flight =
epic `in_progress` or `review` (BOOT active), counting both
main 1:1 epics and wt epics.

---

## 5. 1 human × N agents

| Layer | Rule |
|---|---|
| Human | One operator. Dispatch (`wt add`) and accept (`wt accept`) stay spoken+bin. |
| Dispatch | One epic per isolation checkout. Never two epics in one checkout. |
| Isolation | Worktree **or** clone on `agent/E-n`. |
| Store | Main only. `data/stories.json` stays one file. |
| Agent writes | `src/`, `archive/{packs,evidence}/US-xx-*`. |
| Pull | Everyone: `git pull --ff-only` of **main**. |
| Join | Operator merge of `agent/E-n` at accept. Never squash. |
| Stall | Touch overlap at epic-sign. Not a merge-resolution party. |
| Present | Agent pushes and stops. Operator projects review. |
| WIP | Max 3 in-flight epics after the WIP story. |

---

## 6. Law changes mid-flight

Pack is the contract (laws inlined, archived). Grandfather that
pack. New laws apply to the next pack, not a rebase of a running
agent. If the operator needs them mid-epic: reject, re-pack on
new main, new `wt add`.

---

## 7. Dogfood

Land write-scope verify + `bin/wt add/ls` on this repo as 1:1
(no second agent yet). Then a two-copy arm on a **`bin/init`
throwaway**: two epics, disjoint `scope.touch`, two worktrees,
both PRESENT, operator `wt accept` E-a then E-b (second is a
real merge). No squash. No merge commits on **pull**. Resume
suite item. Until green, WIP stays 1.

---

## 8. Out of this epic

- Story-per-file, claimer, `bin/review` — no rent under this rule.
- E9 official skill after E8 green.
- US-18 workshop. Confirming A-004/A-005/A-006.

---

## 9. Proposed E8 stories (draft only after plan accept)

1. METHOD write-scope + split git rule (ff-only pull vs operator
   join merge). 1:1 unchanged. No bin/wt yet.
2. `bin/verify` FAIL if `agent/*` diff vs main leaves allowed
   prefixes. Adapters: agent must not write the store.
3. `bin/wt add` / `ls` / `present` / `accept` / `reject`. WIP 3
   on `wt add`. `wt accept` = merge then epic-accept semantics.
4. Render In-flight from `bin/wt ls` (fleet). Needs-you still
   review/blocked + review epics.

---

## 10. Closed defaults (revise if you disagree)

1. Isolation unit = **epic**, not story.
2. Store stays **one `stories.json`**. Main is the only writer.
3. Join = **operator merge, no squash**. Pull of main stays
   ff-only.
4. 1:1 does **not** create a worktree.
5. Clones remain legal (same write-scope).
6. Two-agent arm on a throwaway. E9 after E8 green.

Draft E8 stories only after this plan is accepted.
