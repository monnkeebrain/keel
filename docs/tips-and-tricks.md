# Tips & Tricks

Practical patterns from running keel daily. Each entry earned its place by
solving a real incident (IDs reference this repo's failure log).

## Demand the BOOT line, always
`BOOT ok · <phase> · <n ready> · <cue>` is cheaper than debugging drift.
If an agent starts working without emitting it, interrupt and re-boot.
Every protocol deviation we ever caught was visible *before* work started.

## Agent shells have no TTY
Plain `git pull` can hang forever waiting on a pager (F-006). Use
`--no-pager` or export `GIT_PAGER=cat` in inspection commands. The stack
law binds agents to this; humans typing interactively can ignore it.

## Micro-contracts are legitimate
Ceremony scales with blast radius, not with importance. A CSS fix earns:
title + one observable AC + one verification command. A GMP signature flow
earns the full loop. Both are first-class — the gate doesn't care which
regime you're in, only that the contract matches the risk.

## Cold-start testing recipe
1. Fresh clone (or ZIP + `bin/init`).
2. New session, no system prompt, no skills loaded.
3. First message: literally `boot`.
4. Grade: does the BOOT line match `bin/boot` output exactly? Does the agent
   refuse to invent work when nothing is ready?
This single test catches stale adapters, broken discovery paths, and
over-eager agents at once. Run it after any change to METHOD.md.

## Submit evidence before flipping to review
The overview quotes an evidence bundle's opening lines in Needs-you — but
only if the bundle exists when you flip status. Review-without-evidence
renders as silence, and silence teaches reviewers to stop trusting the page.

## Acceptance must be spoken
When an agent works under your identity, acceptance never transfers by
silence, enthusiasm, or elapsed time. The canonical ending is an evidence
table, `PENDING` marks on every DoD that needs a human, and the literal
question: "Accept or reject." Then a human word moves the state.

## The ZIP path is a first-class citizen
Unzipped folder, no `.git`? `bin/init` creates a local repository on `main`
and everything works offline. Add an origin later if sync becomes real.
Onboarding friction is a method bug, not a staff failing.
