# For humans

You are the **operator**. KEEL is the method. The agent is interchangeable.

If you remember one thing: **the quality of the user story is the quality
of the result.** Acceptance criteria (AC) and definition of done (DoD)
are the contract. Vague stories produce vague software. `bin/gate`
refuses the worst of them before anyone writes code.

The full rules are `METHOD.md`. This page is the junior path.

---

## What you do vs what the agent does

| You | The agent |
|---|---|
| Write / sign stories | Boot, pack, implement **one** story |
| Judge `overview.html` | Cite evidence per AC and DoD |
| Say `accept` or `reject` | Stop at review. Never mark `done` |
| Park / reassign if needed | Must not park, reassign, or `git worktree add` |

Chat is a bad place to keep a project. Status, decisions, and “where
we left off” live in this repo. Open a new chat tomorrow, in a
different tool, with a different model. It boots from the files.

---

## First hour

```bash
git clone https://github.com/monnkeebrain/keel.git my-project
cd my-project
bin/init
git add -A && git commit -m "init workspace"
```

`bin/init` clears this repo’s dogfood (stories, decisions, archive)
and creates `src/` for **your** code. Everything outside `src/` is
the method.

1. Write a story in `data/stories.json` (or workshop it with an agent
   using `skills/story-workshop/SKILL.md`). Need: `as_a`, `i_want`,
   `so_that`, at least one AC, at least one DoD. Ban adjectives.
   Say what you would **open tomorrow morning** to check it.
2. `python3 bin/gate US-xx` — fix BLOCKERs.
3. You: `sign US-xx`. The agent may not self-sign.
4. Open the folder in Hermes, Claude Code, Codex, Open WebUI, … and
   say `take US-xx`.
5. The agent claims, packs, works, files evidence, runs `bin/review`.
6. Open `overview.html` (double-click; no server). Check the evidence.
7. You: `accept US-xx` or `reject`.

If nothing is signed and ready, a well-behaved agent **reports and
waits**. That is success, not laziness.

---

## The verbs

| You say | What it means |
|---|---|
| `boot` | Prove you read the repo. Print the BOOT line. |
| `sign US-xx` | Promote a gated story to `ready`. |
| `take US-xx` / `take E-n` | Do this contract (or this epic’s unlocked stories). |
| `accept US-xx` / `accept E-n` | Close it. Evidence required. |
| `reject` | Not done. |
| `park` / `reassign` | Operator only. |

Epics (`E-n`) are a **dispatch envelope**: several stories, one
`take`. Each story still has its own AC, evidence, and accept.

---

## Where to look

- `overview.html` — your dashboard (generated; never hand-edit)
- `STATE.md` — one-screen snapshot
- `data/stories.json` — the contracts
- `archive/evidence/` — proof
- `decisions.md` — why we did it, newest first

---

## Commands you will actually run

`bin/boot` · `bin/gate` · `bin/verify` · `bin/render` · `bin/init`

The agent runs `bin/claim`, `bin/pack`, `bin/review`, and the epic
variants. You run `bin/park`, `bin/reassign`, `bin/epic-accept` if
you need them — the agent must not.

Stdlib Python. No pip. Git is the runtime.

---

## If it feels heavy

Ceremony scales with blast radius. A CSS fix earns one observable AC
and one command that proves it. A payment flow earns the full story.
Both are first-class. See `docs/tips-and-tricks.md`.
