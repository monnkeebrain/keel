# Epic protocol — candidate (not METHOD yet)

Status: plan. Not in force. No `take E-n` until a signed story
lands this in METHOD.md and `bin/`.

This is a Tier B candidate. It does not change invariants.
Anti-overfit: 1:1 friction is planning input, not a FAILURES amendment.

---

## 1. What hurt in 1:1 (field, this repo)

- **Dispatch tax.** `accept US-aa, take US-bb` is two full P-END + P-RUN
  cycles. The human is the scheduler. Independent stories in the same
  slice waited on a chat turn, not on a dependency.
- **Accept grain was right.** Per-story accept caught F-007, fixture
  claims, and “not in the pack.” Do not merge contracts into one
  epic-accept.
- **Pack grain was right.** One compiled story at a time kept scope
  honest. Concatenating seven AC lists would recreate “the whole backlog
  in the prompt.”
- **Isolation is still missing.** One `stories.json` and one working
  copy. Two agents on two epics will collide before any epic protocol
  saves them. That is a *later* epic (file split, worktrees, clerk),
  not a blocker for 1H+1A epic chaining.
- **`epic` is already a label.** E1–E5 exist. They do not dispatch.

---

## 2. Non-goals

- Not an agent orchestrator, queue, or MCP.
- Not one accept for many stories.
- Not one pack containing every AC in the epic.
- Not deleting 1:1. `take US-xx` stays legal forever (micro-contracts,
  hotfixes, the ZIP path).
- Not implementing worktrees / story-per-file / claimer / clerk in the
  MVP. Those are isolation, sequenced after the loop works for 1H+1A.

---

## 3. Definitions

| Term | Meaning |
|---|---|
| **Story** | The contract. AC[], DoD[], accept key, evidence. Unchanged. |
| **Epic** | A dispatch envelope: id, goal, ordered story ids, `depends_on` other epics, `scope`. |
| **Unlocked story** | `ready`, and every `depends_on` story is `done` (accepted). |
| **Take epic** | Claim the epic. Then chain unlocked stories in that epic only, still one story compiled at a time, still no self-accept. |
| **Stall** | Agent cannot start another unlocked story in its epic. Stop and report. Do not invent work in a different epic. |

---

## 4. Invariants that do not move

1. Repo is memory. 2. STATE is O(current). 3. No done without evidence
mapped to AC/DoD. 4. One store, projections elsewhere. 5. IDs immutable.
6. Adapters are generated from METHOD. 7. Complexity pays rent.
8. Human accept. 9. `ff-only`, never merge-by-agent. 10. One writer per
working copy (D-013).

Epic adds **no** new accept authority.

---

## 5. Protocol

### P-EPIC-STORY (planning, human + workshop)

An epic is designed *before* its stories are signed ready.

Required on the epic (proposed store: `data/epics.json`, one object per
id, later maybe `data/epics/E-n.json`):

- `id` (E-n, immutable once referenced)
- `title`, `goal` (one paragraph)
- `stories[]` (ordered US-ids; may be empty while drafting)
- `depends_on[]` (other epic ids; default empty)
- `scope.touch[]` / `scope.forbid[]` (path prefixes, working-copy root)
- `status` (backlog / ready / in_progress / review / blocked / done)

Stories inside the epic gain optional `depends_on[]` (story ids). Missing
= no predecessor. Gate MINOR if an implementation story has neither
`scope` nor `depends_on` and the epic has scope — do not BLOCKER.

**Planning rules (this is how agents do not stall):**

1. **Touch disjointness.** Two epics that are not `done` must not share
   a `scope.touch` prefix. Shared files (`METHOD.md`, `bin/`,
   `data/stories.json`) belong to **at most one** non-done epic.
2. **Cross-epic `depends_on` only onto epics you will finish first.**
   At epic-sign, every id in `depends_on` is `done` *or* the operator
   is signing a sequence in one breath (`sign E7 then E8` with E8
   depending on E7 — E8 stays backlog until E7 is done).
3. **No mid-flight coupling.** If two parallel epics discover they need
   the same file, that is a new integration story in a **third** epic,
   not a silent cross-touch. Do not widen `touch` on an in-progress epic
   without an operator sign.
4. **Within-epic sequence is `depends_on`, not chat order.** If US-28
   cannot start until US-27 is accepted, write it. If they are
   independent, do not fake a chain — the agent will run them back to
   back in one take.
5. **Integration epic last.** Parallel feature epics merge through an
   explicit integration epic whose `depends_on` is those features and
   whose `touch` is the shared surface.

If a requested story cannot be placed without violating (1)–(3), **park
it**. Same as unverifiable work.

### P-EPIC-SIGN

Operator: `sign E-n`. Checks (proposed `bin/epic-gate E-n`):

- Epic has goal, at least one story, scope.touch non-empty for
  implementation epics (docs-only epics may touch `docs/` only).
- No touch overlap with any epic whose status is not `done`.
- Every `depends_on` epic is `done`.
- Every listed story exists; every story’s `epic` field equals this id.
- Story-level `depends_on` ids exist and live in this epic or a `done`
  epic.

Then status `ready`. Same human key as `sign US-xx`.

### P-EPIC-RUN — `take E-n`

1. `git pull --ff-only`. BOOT line (later: include in-flight epic id).
2. Claim epic → `in_progress`. One in_progress epic per working copy.
3. Cover sheet (not a dump): epic goal, story table (id, status,
   unlocked yes/no), epic scope, laws, recent decisions. Then
   `bin/pack` **the first unlocked story** (`--save`).
4. Execute that story inside its pack. Evidence. Status `review`.
   Commit `review US-xx`. Push.
5. **Chain (the 1:1 tax cut):** if another story in *this* epic is
   unlocked, pack it and go to 4 **without a new human take**.
6. **Stall:** no unlocked story left. Report epic id, review list,
   blocked/waiting list. Stop. Do not start another epic. Do not
   self-accept.
7. Session P-END: STATE names the epic and the waiting ids.

Still forbidden: marking `done`; editing outside epic + story scope;
piggyback fixes (D-014).

`take US-xx` remains: claim that story only, no chain.

### Accept

Unchanged and **per story**. Overview Needs-you already stacks.
Epic becomes `done` when every listed story is `done` (mechanical,
`bin/state` / a later `bin/epic-close` check). Reject returns the
story to `ready` or `blocked`; dependents stay locked.

---

## 6. Multi-agent (each agent one epic)

MVP is 1 human × 1 agent × 1 epic chain. Multi-agent is the same
protocol plus isolation already named in METHOD (D-013):

| Layer | Rule |
|---|---|
| Dispatch | One agent `take E-a`, another `take E-b`. Never two epics in one working copy. |
| Isolation | Separate git worktrees (or clones). `ff-only` pull/push. |
| Planning | Touch disjointness (section 5) is what prevents stall, not a runtime lock server. |
| Contract store | Today one `stories.json` — two worktrees claiming different stories still collide on push. **Do not start two-agent dogfood until story-per-file (or equivalent) exists.** |
| Clerk | Status flips through `bin/` (`claim`, later `review`). Hand-edits of the store from execute agents stay a later clerk story. |
| Human | One accept queue. WIP law: default max two in-flight epics because one human cannot review eight. |

If agent B would need a file agent A has in `touch` and A is not done:
**B’s epic was mis-planned.** Park B. Do not wait at runtime.

---

## 7. STATE / overview (projections)

- STATE: in-flight epic id(s), unlocked counts, waiting-on-accept ids.
  Still O(current). Resume cue can stay one sentence; the list is the
  machine truth.
- Overview: group stories by epic; Needs-you stays flat (human queue).
  No new JS framework.

---

## 8. What we abandoned (and why it comes back)

US-27..30 (worktree flag, claimer ids, story-per-file, clerk-only) were
unsigned isolation stories. Operator dropped them so this plan could
go first. IDs stay (`abandoned`). They return as **E8 isolation**,
after E7 (protocol MVP) has been dogfooded 1H+1A.

Do not resurrect them inside E7. E7 must be shippable on today’s
single `stories.json`.

---

## 9. Dogfood on this repo

**E7 — Epic protocol MVP** (serial, this working copy)

Proposed stories (draft only after this plan is accepted):

1. METHOD §3 P-EPIC-* text (no bin). Laws stay short.
2. `data/epics.json` + `bin/epic-gate` / `bin/epic-claim` / cover sheet
   + chain rule in pack/adapter. `take E-n` documented.
3. Optional story `depends_on[]`; claim/pack refuse if predecessor
   not `done`. AMENDMENTS.
4. Overview groups by epic; STATE lists in-flight epic.
5. Cold-start arm: `take E7` chains two independent tiny stories,
   stalls on a third that `depends_on` the first, human accepts,
   `take E7` continues. Resume suite item.

**First use of the loop:** one real slice (e.g. workshop US-18 inside
an E-workshop *after* E7 is done), not mixed into E7.

**E8 — Isolation for 1H+NA** (only after E7 cold-start is green)

Story-per-file → claim `--worktree` → claimer ids → clerk/`bin/review`.
In that order: files first (git can merge/ff distinct paths), then
worktrees, then identity, then write-set.

**E9 — Official skill** (adapter, after E7 is in METHOD)

Thin, model-agnostic, in repo `skills/`. Describes 1:1 *and* `take E-n`.
Not a copy of the Hermes skill.

---

## 10. Open questions (operator)

Answer these before drafting E7 stories. Defaults in parentheses.

1. Epic store: one `data/epics.json` vs `data/epics/E-n.json`?
   (Recommend one JSON file until story-per-file exists — same collision
   budget as today, 1H+1A only.)
2. Chain after `review` without waiting for accept when `depends_on`
   is empty — confirm? (Recommend yes. That *is* the tax cut.)
3. WIP: max in-flight epics = 1 until E8, then 2? (Recommend yes.)
4. May `take E-n` run if some listed stories are still `backlog`?
   (Recommend yes: chain whatever is unlocked; stall when nothing is.)
5. Boot line: add epic id when in_progress, or keep STATE-only?
   (Recommend STATE + cover sheet first; boot line later if we miss it.)

---

## 11. Complexity rent

Epic pays rent if and only if: one `take E-n` can land ≥2 independent
stories in one session without a human turn between them, and a
mis-planned cross-touch is rejected at **sign**, not at Friday 4pm.

If we cannot state that in METHOD in less than a page, the protocol is
too fat. Cut before coding.
