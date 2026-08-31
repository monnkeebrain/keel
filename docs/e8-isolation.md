# E8 — Agent loop (final plan, not METHOD yet)

Status: replaces the worktree-CLI candidate. Not in force until
signed stories land in METHOD.md and `bin/`.

Tier B. Invariants do not move. Abandoned US-27..30 stay
abandoned. Those ids are not reused.

E7 is accepted. Epic dispatch works for 1H×1A×1 cwd.
See `docs/system.md` for the tower.

---

## 1. What we were about to get wrong

The previous E8 drafts (`bin/wt`, path-import, `agent/E-n`
branches, scratch store vs landed store, sibling worktree paths)
were **harness engineering**. KEEL is harness-agnostic. Hermes,
Claude Code, Codex, Open WebUI already spawn subagents and open
folders. Teaching KEEL to create worktrees does not make better
contracts. It makes a second, worse harness.

Dogfood: the method paid rent when it was **BOOT, pack, evidence,
human accept**. It cost rent when we JSON-edited status, piggybacked
fixes, or loaded more protocol than the pack.

---

## 2. Load-bearing rule

> **KEEL does not spawn agents. It signs work, packs one story,
> and refuses unevidenced done.**

The cwd is an input. The harness or the operator provides it.
`take E-n` is how an agent is delegated an epic. `take US-xx`
remains the hotfix.

Write-scope (already packed): execute agents write `src/` and
`archive/{packs,evidence}/`. They do not write METHOD or `bin/`
unless the story is about those paths. Status moves through `bin/`.

---

## 3. What actually helps an agent (this repo, this loop)

| Keep | Rent |
|---|---|
| BOOT line, no invent | one glance |
| pack / epic-pack | working set; laws inlined |
| gate on stories | human output quality-checked before build |
| claim | `in_progress` is real |
| overview for humans | I do not sell in chat |
| operator accept | the gate that held on every model |
| decisions newest-first | accretion into the next pack |
| one writer per cwd | no silent clobber |

| Missing | Rent |
|---|---|
| `bin/review US-xx` | twin of claim; stop hand-editing JSON to flip review |

| Do not build | Why |
|---|---|
| `bin/wt *` | harness job |
| path-import / join merge | not a method problem if KEEL does not own branches |
| claimer schema | `CLAIM` already prints root + HEAD |
| story-per-file now | no second cwd in anger yet |
| WIP 3 now | WIP 1 matches one cwd; raise when two cwds exist |
| fat official skill now | would freeze a 1:1 ritual; E9 after this loop is boring |

---

## 4. E8 scope (thin)

**One story, when you accept this plan:**

- `bin/review US-xx`: `in_progress` → `review`, print `REVIEW US-xx`.
  Refuse otherwise, store unchanged.
- Pack write-set: status via `bin/claim` and `bin/review` only.
  Status `done` still forbidden. Park/reassign/epic-accept still
  operator-only.
- METHOD P-RUN: after evidence, `bin/review`, then stop.
- Wrapper `bin/review`, same pattern as `bin/claim`.
- 1:1 behavior otherwise unchanged. No worktree. No new schema.

**METHOD sentence (same story or a two-line sibling):**
the operator/harness may open another cwd (worktree or clone).
The agent does not create one. Adapters: do not run
`git worktree add` as a KEEL verb.

---

## 5. Later, failure-driven (not this epic)

When two cwds actually collide on `stories.json`:

- split to `data/stories/US-xx.json` (one canonical store, like
  epics).
- raise `epic-claim` WIP from 1 to 3.

Not before. That is P5, not a fleet product.

---

## 6. Out of E8

- `bin/wt`, worktree path policy, GUI sibling folders
- MCP, orchestrator, message bus
- E9 official skill (thin, after `bin/review` is boring)
- US-18 workshop UX
- Confirming A-004/A-005/A-006 as a passenger

---

## 7. Stories (draft only after you accept this plan)

1. METHOD P-RUN names `bin/review`; cwd is given, not created;
   adapters forbid worktree-add as method.
2. `bin/review` + wrapper + pack write-set text.

Accept this retraction or name what still requires a KEEL-owned
worktree CLI.
