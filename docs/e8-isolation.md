# E8 — Isolation (candidate, not METHOD yet)

Status: plan, revised after method review. Not in force. No store
cutover, no WIP-3, no worktree flag until signed E8 stories land in
METHOD.md and `bin/`.

Tier B candidate. Invariants do not move. Abandoned US-27..30 stay
abandoned (IDs immutable). E8 gets **new** story ids.

E7 is accepted. Dispatch works for **1 human × 1 agent × 1 working
copy**. Serial model-swap already works. Parallel agents do not.

---

## 1. What is actually broken

Two execute agents in two processes both rewrite `data/stories.json`.
`git pull --ff-only` + never-merge rejects the second push.

Epic files are already one-per-id. Stories are not.

That is not the only shared write. Even after a story split, both
agents still rewrite **`STATE.md` and `overview.html`** on every
P-END. Those are projections (invariant 4). Two regenerations of the
same store still conflict on `generated-at`. Isolation that ignores
projections is a ghost.

CLAIM already prints working-copy root and short HEAD (US-20).
One writer per working copy is already METHOD (US-25). E8 does not
reverse that. It gives each agent its own copy, and stops execute
copies from committing projections.

Topology A — serial swap: already works. No new machinery.
Topology B — two agents, one working copy: still forbidden.
Topology C — two agents, two working copies + ff-only: E8 target.

---

## 2. Invariants (do not move)

1. Everything that matters lives in this repo.
2. STATE stays O(current) — do not log worktree history there.
3. No done without evidence.
4. One canonical store. `data/stories/` replaces `data/stories.json`.
   Two stores at once is a verify FAIL. Pages stay projections.
5. Raw history → archive/; lessons → decisions.md.
6. IDs immutable. Do not reuse US-27..30.
7. Adapters generated from METHOD. Reading-order path updates with
   the store cutover, then `bin/adapt`.
8. Complexity pays rent.

Human accept. One packed story per agent. ff-only, never merge.
Park / reassign / epic-accept operator-only. Touch disjointness at
epic-sign. Stall is a planning bug, not a chat-wait.

---

## 3. Non-goals

- Not an orchestrator, queue, bus, agent-to-agent chat, or MCP.
- Not a second database. Git remains the runtime.
- Not requiring a worktree for 1:1 (US-25 stands).
- Not deleting `take US-xx`.
- Not wrapping `git worktree add` in `bin/` (git already exists;
  agents already run it). Isolation is a METHOD rule, not a flag.
- Not schema `claimer.*`. CLAIM root+HEAD + `git worktree list`
  are the live map. A field would need an amendment; A-004/A-005/
  A-006 are already TRIAL.
- Not `bin/accept` / `bin/reject` / `bin/end` / `bin/worktree`.
- Not a fat official skill (E9). Not US-18. Not design MCP.

---

## 4. bin/ that pay rent vs bin/ that do not

Agents burn tokens hand-editing JSON. Deterministic `bin/` that
flips one status on one file is the KEEL shape (claim, park,
epic-present). Wrapping git is not.

| Keep / add | Rent |
|---|---|
| Story-per-file (`load`/`save` directory) | Two agents can land. Same shape as `data/epics/`. |
| `bin/review US-xx` | Twin of `bin/claim`. in_progress → review. Execute agents stop hand-editing the store. |
| Pack Do-not-write `data/stories/` | Same as today's METHOD.md / bin/ fence. |
| `epic-claim` WIP 3 | One condition. After isolation, not before. |
| `verify` FAIL if `data/stories.json` still exists | Two stores forbidden. |
| `init` wipe `data/stories/*.json`, keep dir | Same as epics. |

| Drop | Why |
|---|---|
| `claim --worktree` | Git already adds worktrees. Flag implies path/branch/cleanup policy. No FAILURES entry. |
| `claimer.id` / `claimer.worktree` | Schema for a `git worktree list` line. |
| `bin/worktree`, `bin/end`, `bin/accept` | Status-loop speech + git stay as they are. |

---

## 5. Projection rule (the miss in the first draft)

Invariant 4: STATE and overview are projections, regenerated freely.

- **1:1** (this working copy is the one the operator reads): keep
  today's P-END — `bin/state` && `bin/render` && commit them.
- **Linked worktree** (`git-dir` ≠ `git-common-dir`): commit
  `data/stories/US-xx.json`, `data/epics/E-n.json` if touched,
  `archive/`. Do **not** commit `STATE.md` or `overview.html`.
  Operator copy (main) pulls, then `bin/state && bin/render` once.

Detect with git, not a new schema field. 1:1 does not get more
human toil. N does not fight over timestamps.

`bin/review` only flips the story. It does not write STATE.
Same as `bin/claim`.

---

## 6. Order (load-bearing)

1. **Story-per-file.** `data/stories/US-xx.json`.
   `data/stories.json` gone. Every `bin/` that reads stories reads
   the directory. METHOD §2 + §0 path. `bin/adapt`. No two-agent
   dogfood until this is accepted.
2. **Clerk + projections.** `bin/review`. Pack Do-not-write
   `data/stories/`. METHOD: worktree P-END omits projections.
3. **WIP 3.** `bin/epic-claim` refuses when in-flight epics
   (`in_progress` or `review` — same set BOOT already calls
   active) would exceed 3. Until this lands, WIP stays 1.

Worktree *use* is documented in METHOD/tips in story 2. No flag.

---

## 7. 1 human × N agents

| Layer | Rule |
|---|---|
| Human | One operator. Accept / park / reassign / epic-accept stay spoken. |
| Dispatch | Agent A `take E-a`, agent B `take E-b`. Never two epics in one working copy. |
| Isolation | One working copy per in-flight epic (worktree or clone). |
| Store | `data/stories/US-xx.json` + `data/epics/E-n.json`. |
| Landing | Each copy `git pull --ff-only` then `git push`. Never merge. |
| Projections | Committed on the copy the operator reads (main). |
| Stall | Touch overlap at epic-sign → do not start the second epic. |
| Present | Each agent presents *its* epic. |
| WIP | Max 3 in-flight after story 3. Human review budget, not a scheduler. |

N can grow later by raising the WIP number in a signed story.
Do not add a config file or STATE field for it.

---

## 8. METHOD / bin/ deltas (when stories exist)

Existing to change: every command that calls `load_stories` /
`save_stories` (gate, claim, pack, state, render, verify, init,
boot, epic-*, park, reassign, epic-accept).

New: `bin/review` only.

Not new: `claim --worktree`, `bin/worktree`, `claimer`, `bin/end`.

---

## 9. Dogfood

Land 1 and 2 on **this** method repo as 1:1 (the cutover is the
migration). Then a two-copy arm:

- Prefer a **`bin/init` throwaway** so method history stays linear
  (no fixture epics on `main`).
- Grade: two worktrees, two tiny disjoint epics, both claims land,
  both presents visible after main `bin/render`, human `accept E-a`
  then `accept E-b`. No merge commits.
- Resume-suite item. Until that arm is green, do not raise WIP to 3
  on a staff project.

---

## 10. Out of this epic

- **E9** — thin official skill after isolation is real.
- **US-18** — workshop depth. First *use*, not isolation.
- **Confirming A-004/A-005/A-006** — separate, not a passenger.
- **Cold-start matrix** — rerun after the store path changes.

---

## 11. Closed defaults (revise if you disagree)

1. Sequence: file → clerk/projections → WIP 3. **Drop claimer.**
2. Worktree: **document `git worktree add` only.** No `--worktree` flag.
3. WIP 3 counts **`in_progress` + `review`** (BOOT active).
4. Two-agent arm on a **throwaway `bin/init`**, after 1–2 land here.
5. **E9 after E8 green**, not before.

Draft E8 stories only after this plan is accepted.
