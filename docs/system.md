# KEEL as a system (agent-facing)

Not METHOD. Narrative. METHOD.md wins if they disagree.

KEEL is not a harness, an orchestrator, or a way to spawn agents.
Harnesses already do that. KEEL is a **method in a git repo**: the
human’s intent becomes a signed contract; an agent executes one
pack; a human accepts on evidence. Deterministic `bin/` verbs so
the contract does not drift with the model.

The future user is more often an agent. The human remains the
**accept key** and the **author of contracts**. Quality of stories
is the quality of the product.

---

## Tower (one job per layer)

```
human intent
  → story (contract) → gate
    → epic (optional dispatch envelope)
      → pack (the only working set)
        → src/ + archive/evidence (work)
          → review (bin/) → overview (human)
            → accept (human speech) → decisions (accretion)
```

| Layer | Job | Agent reads | Agent writes |
|---|---|---|---|
| git | runtime | pull ff-only | commit/push |
| METHOD + laws | standing world | pack inlines laws; METHOD §1 at boot | never, unless the story is about them |
| data/stories, data/epics | contracts | pack, gate, boot | **only via bin/** |
| bin/ | verbs | run them | n/a |
| STATE, overview | projections O(current) | boot / STATE | via `bin/state`, `bin/render` |
| archive | write-once raw | evidence paths | packs, evidence |
| decisions | distilled lessons | last ~5 in pack | append on accept (main) |
| adapters | compiled METHOD | BOOT ritual | never (generated) |
| src/ | the project | as packed | yes |
| harness | cwd, tools, subagents | outside KEEL | outside KEEL |

If a problem is “how do I spawn the other agent?”, it is **not a
KEEL problem**.

---

## What an execute agent actually needs

1. `bin/boot` — one line: may I work, on what, stop if nothing ready.
2. `bin/pack` / `bin/epic-pack` — one story, laws, write-set,
   do-not-write, recent decisions. Do not reload METHOD.
3. Work inside the write-set. Evidence per AC. Unevidenced done is void.
4. Flip status with **bin/**, not by editing JSON.
5. Stop. Present. Do not accept, park, reassign, or spawn work.

Token budget is the pack. Accretion is `decisions.md` + `laws/`,
not a hidden memory, not a worktree fleet.

---

## Parallel agents

One writer per working copy (already METHOD). A working copy is
whatever **cwd** the harness or operator opened.

Two agents ⇒ two cwds (worktree or clone), created **outside**
KEEL. Each boots, each `take E-n` (different epics, disjoint
touch). `git pull --ff-only`. KEEL does not `git worktree add`.

Until two cwds exist in anger, do not split `stories.json` and
do not raise WIP. Complexity pays rent.

---

## Human surface

`overview.html` is for the operator. Agents do not perform for
chat. Contracts live in `data/`. Judgment lives on the overview.
