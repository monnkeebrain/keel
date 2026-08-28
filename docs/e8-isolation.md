# E8 — Isolation (candidate, not METHOD yet)

Status: plan. Not in force. No worktree, no story-per-file, no WIP-3
until signed E8 stories land in METHOD.md and `bin/`.

This is a Tier B candidate. Invariants do not move.
Anti-overfit: wanting two agents is planning input, not a FAILURES
amendment. Abandoned US-27..30 stay abandoned (IDs immutable). E8
gets **new** story ids.

E7 (US-31..36) is accepted. Dispatch works for **1 human × 1 agent ×
1 working copy**. Serial model-swap already works. Parallel agents
do not.

---

## 1. What E7 did not buy

- **One JSON array.** Two claims in two processes still rewrite
  `data/stories.json`. `git pull --ff-only` + never-merge means the
  second push is rejected. Epic files are already one-per-id; stories
  are not.
- **WIP 1.** `bin/epic-claim` refuses a second `in_progress` epic.
  That is the 1H+1A safety rail. 1H+N needs a higher cap **after**
  isolation exists.
- **CLAIM already prints** working-copy root and short HEAD (US-20).
  A later worktree can bind without a new schema field.
- **One writer per working copy** is already METHOD (US-25). E8
  does not reverse that. It gives each agent its own copy.

Topology A — serial multi-agent (swap Grok for Claude tomorrow):
already works. Do not add machinery.

Topology B — two execute agents, one working copy: still forbidden.

Topology C — two execute agents, two working copies (worktrees or
clones) + ff-only: the E8 target.

---

## 2. Non-goals

- Not an orchestrator, queue, message bus, agent-to-agent chat, or MCP.
- Not a second database. Git remains the runtime.
- Not requiring a worktree for 1:1 sessions (US-25 stands).
- Not deleting `take US-xx`. Hotfixes stay legal during an epic.
- Not reusing US-27..30. Those ids stay `abandoned`.
- Not a fat official `skills/` dump (that is E9).
- Not US-18 (workshop UX). Not design-system MCP.
- Not schema `claimer.id` unless CLAIM root+HEAD plus `git worktree
  list` prove insufficient. Complexity must pay rent.

---

## 3. What must not change

Human accept. One packed story per agent. Evidence mapped to AC/DoD.
`git pull --ff-only`, never merge. STATE stays O(current). IDs
immutable. Adapters generated from METHOD. Park / reassign /
epic-accept operator-only. Touch disjointness at epic-sign (stall is
a planning bug, not a chat-wait).

---

## 4. Order (load-bearing)

Worktrees on today's `stories.json` still collide. So:

1. **Story-per-file** — `data/stories/US-xx.json`. Two status writes
   no longer share a file. `data/stories.json` goes away (not a
   second store). `bin/init` empties the directory. `bin/verify`
   FAIL on duplicate ids. Every current `bin/` command that reads
   stories reads the directory.
2. **Clerk (`bin/review`)** — executing agents stop hand-editing
   the contract store. `bin/review US-xx` is the in_progress →
   review flip. Pack Do-not-write lists `data/stories/`. Claim
   remains the only command that sets `in_progress`.
3. **Optional worktree** — `bin/claim US-xx --worktree` *or*
   documented `git worktree add` (operator choice below). 1:1 still
   does not create one. Isolation = worktree **or** separate clone,
   then ff-only / push.
4. **WIP 3** — `bin/epic-claim` refuses when in-flight epics would
   exceed 3. In-flight = `in_progress` or `review` (same set BOOT
   already calls active). Until this lands, WIP stays 1.

Do not dogfood two agents until (1) is accepted.

---

## 5. Why this order, and what we drop

**Story-per-file first.** US-29 was right; it was early. Epic files
already proved one-json-per-id. Stories copy that shape.

**Clerk next.** Today every run hand-edits `stories.json` to flip
review. After the split, that is one file — still a protocol hole.
`bin/review` is the missing twin of `bin/claim`. Small. Pays rent.

**Worktree after the store can take two writers.** A flag that
shells `git worktree add` is convenience, not isolation by itself.

**Drop schema claimer** (abandoned US-28) unless dogfood shows
STATE cannot name the holder. CLAIM already prints root + HEAD.
`git worktree list` is the live map. A new optional field needs an
amendment; A-004/A-005/A-006 are already TRIAL.

**WIP 3 last.** Mechanical. Dangerous before isolation.

---

## 6. Multi-agent shape (1 human × N agents)

| Layer | Rule |
|---|---|
| Human | One operator. Accept / park / reassign / epic-accept stay spoken. |
| Dispatch | Agent A `take E-a`, agent B `take E-b`. Never two epics in one working copy. |
| Isolation | One working copy per in-flight epic (worktree or clone). |
| Store | `data/stories/US-xx.json` + `data/epics/E-n.json`. |
| Landing | Each copy `git pull --ff-only` then `git push`. Never merge. |
| Stall | Touch overlap at epic-sign → do not start the second epic. Do not wait on the other agent's files. |
| Present | Each agent presents *its* epic. Human may accept A and partial-B in one sitting. |
| WIP | Max 3 in-flight epics after the WIP story. Human review budget, not a cluster scheduler. |

---

## 7. METHOD / bin/ deltas (when stories exist)

Existing to change: `gate`, `claim`, `pack`, `state`, `render`,
`verify`, `init`, `boot` (story load path), `epic-*` (story lookup),
`park`, `reassign`, `epic-accept`.

New: `bin/review`. Optional: `claim --worktree`.

Not a new command: `bin/worktree`, `unpark`, `claimer`, orchestrator.

---

## 8. Dogfood

After story-per-file + clerk: one operator, two worktrees, two
tiny disjoint epics on **this** method repo *or* a throwaway
instance (`bin/init`). Grade: both claims land, both presents
reach overview, human `accept E-a` then `accept E-b` (or partial).
No merge commits. Resume suite item.

Until that arm is green, do not raise WIP to 3 on a staff project.

---

## 9. Out of this epic

- **E9** — thin official skill in `skills/` after isolation is real.
  Freezing a 1:1 skill was the miss we avoided before E7.
- **US-18** — workshop depth. First *use*, not isolation.
- **Cold-start matrix** — rerun after E8 store cutover (adapters
  mention `data/stories.json` today).

---

## 10. Open questions (operator)

1. Sequence above (file → clerk → worktree → WIP 3) and **drop
   claimer** — confirm or revise.
2. Worktree: `bin/claim --worktree` convenience flag, or document
   `git worktree add` only (no new flag)?
3. WIP 3 counts `in_progress`+`review` (BOOT active), or
   `in_progress` only (a presented epic does not fill a slot)?
4. E8 dogfood on this repo, or on a `bin/init` throwaway so method
   history stays linear?
5. E9 (official skill) after E8 green, not before?

Draft E8 stories only after this plan is accepted.
