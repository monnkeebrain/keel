# Epic protocol — candidate (not METHOD yet)

Status: plan, operator answers folded in. Not in force. No `take E-n`
until a signed story lands this in METHOD.md and `bin/`.

This is a Tier B candidate. It does not change invariants.
Anti-overfit: 1:1 friction is planning input, not a FAILURES amendment.

---

## 1. What hurt in 1:1 (field, this repo)

- **Dispatch tax.** `accept US-aa, take US-bb` is two full P-END + P-RUN
  cycles. Independent stories in the same slice waited on a chat turn.
- **Accept grain was right for defects.** Per-story evidence caught
  F-007 and fixture claims. Do not merge AC lists. Batch *judgment*
  of already-evidenced stories is a different thing (section 5).
- **Pack grain was right.** One compiled story at a time. Do not dump
  every AC in the epic into one prompt.
- **Isolation is still missing** for two agents (`stories.json`, one
  working copy). That is E8, not a blocker for 1H+1A epic chaining.
- **`epic` is already a label.** E1–E5 exist. They do not dispatch.

---

## 2. Non-goals

- Not an agent orchestrator, queue, or MCP.
- Not one pack containing every AC in the epic.
- Not deleting 1:1. `take US-xx` stays legal (micro-contracts, hotfixes).
- Not implementing worktrees / story-per-file / claimer / clerk in E7.
- Not building on unaccepted work: `depends_on` still means predecessor
  is `done`, not `review`.

---

## 3. Definitions

| Term | Meaning |
|---|---|
| **Story** | The contract. AC[], DoD[], evidence. Human still owns `done`. |
| **Epic** | Dispatch envelope: id, goal, ordered story ids, `depends_on` epics, `scope`. One file: `data/epics/E-n.json`. |
| **Unlocked** | Story is `ready`; every story `depends_on` is `done`; story is listed on this epic. |
| **Logical to proceed** | Unlocked, and the work so far has not made the next story false or out of scope. If it has: **stop and present** — do not park, do not invent a replacement. |
| **Take epic** | Claim the epic. Chain every unlocked story that is logical. Evidence each → `review`. Present the epic. Do not self-accept, park, or reassign. |
| **Present epic** | No further unlocked+logical story. Epic → `review`. Cover: story table, evidence paths, waiting list, *recommended* parks. |
| **Park / reassign** | **Operator only.** `park US-xx` or `reassign US-xx`. The agent may recommend; it must not change those statuses. |
| **Accept epic** | Legal only when every id still listed on the epic is `done`. May be combined with batch-accept of its `review` stories. |

---

## 4. Invariants that do not move

Repo is memory · STATE O(current) · no done without evidence mapped to
AC/DoD · one store, projections elsewhere · IDs immutable · adapters
from METHOD · complexity pays rent · **human accept** · `ff-only` ·
one writer per working copy (D-013).

`accept E-n` is still a **human** speech act. It does not let the agent
close work. It lets the human close a *batch* they have judged.

---

## 5. Protocol

### P-EPIC-STORY (planning)

Epic designed before its stories are signed ready.

Store: **`data/epics/E-n.json`** (one object, one file). Repo/project
native; two agents on two epics do not edit the same epic file.
(Story files remain `data/stories.json` until E8.)

Fields: `id`, `title`, `goal`, `stories[]`, `depends_on[]` (epic ids),
`scope.touch[]` / `scope.forbid[]`, `status`.

Stories gain optional `depends_on[]` (story ids).

**Planning rules (stall prevention):**

1. **Touch disjointness.** Two epics that are not `done` must not share
   a `scope.touch` prefix. `METHOD.md`, `bin/`, `data/stories.json`
   belong to at most one non-done epic.
2. **Cross-epic `depends_on` only onto epics you finish first** (already
   `done`, or signed later and left backlog until then).
3. **No mid-flight coupling.** Shared need → new integration epic, not
   a widened `touch` on an in-progress epic.
4. **Within-epic sequence is `depends_on`.** Hard wait = predecessor
   `done`. Independent stories are chained in one take.
5. **Integration epic last.**

If a story cannot be placed without violating 1–3: **park**.

### P-EPIC-SIGN

`sign E-n`. Proposed `bin/epic-gate E-n`:

- Goal, ≥1 story, scope.touch set for implementation epics.
- No touch overlap with any epic not `done`.
- Every `depends_on` epic is `done`.
- Listed stories exist; each story’s `epic` field is this id.
- Story `depends_on` ids exist (this epic or a `done` epic).

Stories on the epic should already be `ready` (signed) *or* the take
will simply skip `backlog` ones (park / wait). Prefer sign stories
before or with the epic so `take E-n` can finish the slice.

Then epic `ready`.

### P-EPIC-RUN — `take E-n`

1. `git pull --ff-only`. BOOT line includes **active epic id(s)**.
2. Claim epic → `in_progress`. WIP: **max 1** in-flight epic per
   working copy until E8; then **max 3** (human review budget).
3. Cover sheet: goal, story table (status, unlocked, logical?), scope,
   laws, recent decisions. Pack **one** unlocked story (`--save`).
4. Execute. Evidence. Story → `review`. Commit `review US-xx`. Push.
5. **Chain:** if another story in this epic is unlocked **and logical
   to proceed**, pack it and go to 4. No new human `take`.
6. **Not logical:** stop. Do not park, do not reassign, do not invent
   a replacement. Include a recommended park/reassign in the present.
7. **Present:** no remaining unlocked+logical story. Epic → `review`.
   Report: evidence index (every `review` story + path), waiting-on
   predecessor list, leftover `backlog`, recommended parks.
   Stop. Do not mark stories or epic `done`.
8. P-END: STATE names active epic(s) and the present/waiting ids.

`take US-xx` remains: that story only, no chain, no epic present.

Hard `depends_on`: US-28 waiting on US-27 stays locked until US-27 is
**accepted** (`done`). The agent presents the epic with US-27 in
`review` and US-28 waiting. Human accepts US-27 (or the batch); a
later `take E-n` continues. That is not a planning stall; it is the
accept key.

### Accept (stories and epic)

The agent presents a completed *attempt* (all it could legally run).
The operator judges.

**`accept E-n`** (happy path)

- Every listed story is `review` or already `done`; none waiting on
  a predecessor inside the epic; none still `in_progress`.
- Effect: all `review` stories on that epic → `done`; epic → `done`.
- One speech act, one commit (or one per story plus epic — mechanical
  detail for E7). Evidence already filed per story.

**Partial**

- `accept US-aa US-bb` — those `review` stories → `done`. Epic stays
  `review` or `in_progress`.
- `park US-cc` — operator only. Story → `blocked` (stays on the epic
  until reassigned or later unblocked). Agent recommendations are not
  a status change.
- `reassign US-cc` — operator only. Off this epic’s `stories[]`.
  Destination: `ready` on this epic, `backlog`, or another epic id.
  Evidence stays in `archive/` (IDs immutable).
- Epic **`accept E-n` is illegal** until every id **still listed**
  on the epic is `done` (blocked stories must be reassigned or
  unblocked and completed first).

**Reject** a story: back to `ready` or `blocked`; dependents stay
locked. Epic not accepted.

Overview: Needs-you lists `review` stories **and** epics in `review`
(accept-all vs partial). Checkboxes stay session-local. Image evidence
unchanged.

---

## 6. Multi-agent (each agent one epic)

| Layer | Rule |
|---|---|
| Dispatch | Agent A `take E-a`, agent B `take E-b`. Never two epics in one working copy. |
| Isolation | Worktrees/clones + `ff-only`. E8. |
| Planning | Touch disjointness. Mis-planned overlap → park the second epic, do not wait. |
| Epic files | `data/epics/E-n.json` — two epic-claims do not share a file. |
| Story files | Still one `stories.json` until E8. **No two-agent dogfood until story-per-file.** |
| WIP | 1 in-flight epic until E8, then max **3**. |
| Present | Each agent presents *its* epic. Human may `accept E-a` and partial `E-b` in one sitting. |

---

## 7. STATE / boot / overview

- BOOT: `BOOT ok · keel v… · <phase> · <n ready> · epics <ids or none> · <cue>`
  Active = epic status `in_progress` or `review`.
- STATE: those ids, unlocked counts, waiting-on-accept story ids.
  O(current).
- Overview: group stories by epic (required, human surface). Needs-you
  includes epic `review` rows plus story rows. No new JS framework.

---

## 8. Abandoned US-27..30

Isolation stories stay `abandoned`. Return as **E8** after E7 dogfood.
E7 ships on today’s `stories.json` plus **new** `data/epics/E-n.json`.

---

## 9. Dogfood

**E7 — Epic protocol MVP** (1H+1A, this working copy)

Draft only after operator accepts this plan:

1. METHOD §3 P-EPIC-* (including present + `accept E-n` / reassign).
2. `data/epics/E-n.json` + `bin/epic-gate` / `bin/epic-claim` /
   cover sheet + chain + present. Boot line lists active epics.
3. Story `depends_on[]`; pack/claim refuse unless predecessor `done`.
4. `accept E-n` batch-closes review stories + epic when legal;
   operator-only `park` / `reassign`; overview **groups stories by epic**.
5. Cold-start: `take E7` chains two independent stories, presents
   (does not park) if a third is not logical or has `depends_on`.
   Human `accept E7` *or* partial + park/reassign. Resume suite item.

**E8 — Isolation** (after E7 green): story-per-file → worktree →
claimer → clerk. WIP cap becomes 3.

**E9 — Official skill** after E7 is in METHOD.

**US-18** first *use* of the loop, not inside E7.

---

## 10. Closed questions

| # | Decision |
|---|---|
| 1 | One file per epic: `data/epics/E-n.json`. |
| 2 | Chain after `review` when `depends_on` is empty. Yes. |
| 3 | Max in-flight epics = 1 until E8, then **3**. |
| 4 | Unlocked **and logical** → proceed; else **present** (operator parks). |
| 5 | BOOT line includes active epic ids. |

---

## 11. Complexity rent

Epic pays rent if: one `take E-n` can evidence ≥2 independent stories
without a human turn between them, the human can `accept E-n` once,
and a bad cross-touch dies at **sign**.

If METHOD needs more than a page for P-EPIC-*, cut before coding.
