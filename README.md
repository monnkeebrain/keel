# KEEL

A file-native method for human–machine delivery.

One git repo per project. The repo holds everything that matters:
a standing world-model (`laws/`), signed intents (`data/stories.json`),
append-only residue (`decisions.md`), a bounded live snapshot (`STATE.md`),
and dumb CLI mechanics (`bin/`). Any agent harness — Hermes, Open WebUI +
Open Terminal, Codex, OpenCode — plugs in through thin generated adapters.
Nothing important lives in a harness; harnesses churn, files persist.

## Core laws (short form)

1. Files over harness. If it matters, it is in the repo.
2. State is O(current). History is archived and distilled, never re-read wholesale.
3. Evidence or it didn't happen. Done is claimed by verification, not vibes.
4. One canonical store, many projections. Pages are generated or they are lies.
5. Complexity must pay rent — in the method itself too.

Full text lives in [METHOD.md](METHOD.md). Evolution rules live in
[AMENDMENTS.md](AMENDMENTS.md). Roadmap lives in [PLAN.md](PLAN.md).

## Status

Phase 0 — method repo initialized. No resume test passed yet.
The method starts existing when a cold session resumes work from this
repo without asking a single clarifying question.
