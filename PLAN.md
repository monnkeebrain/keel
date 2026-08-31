# KEEL — PLAN

Working plan v1. Corrections absorbed: multi-instance Open WebUI fleet,
skills supported natively in both primary harnesses, amendment governance
against method decay.

## Scope decisions

- Primary harnesses (dogfood): **Hermes Agent**, **Open WebUI + Open Terminal**
  (multiple Docker instances across VPS/local machines/servers).
- Secondary validation: **Codex / OpenCode** (staff daily drivers).
  They read plain `AGENTS.md`, so this requires zero extra code if Phase 1
  is done right. If it needs engineering, Phase 1 has a bug.
- Deferred until further notice: MCP servers, additional harnesses,
  any UI beyond generated static pages.

## Phases

### P0 — Method repo (days)
This repository. Seed artifacts exist; METHOD.md skeleton written.
- Exit gate: METHOD.md §Invariants + §Protocols filled in; no section
  longer than needed to be unambiguous.

### P1 — Mechanical core (1–2 weekends)
`bin/` tools in pure Python stdlib only (must run in OT slim containers,
on VPSes, anywhere):
- `gate check <story-id>` — ready-gate checklist over stories.json
- `pack compile <story-id>` — context pack: story + law slices + recent decisions
- `state snapshot` — regenerate bounded STATE.md from repo state
- `distill` — extract decision candidates from archive into decisions.md
Adapter generator: emits `adapters/AGENTS.md`, `.hermes.md`,
`owui-system-prompt.md` from METHOD.md. Generated, never hand-edited.
- Exit gate: all commands run identically via Hermes terminal tool and
  an Open Terminal shell. Zero pip installs required.

### P2 — Two-harness dogfood (the real gate) ✅ COMPLETE 2026-08-25
Matrix green: OWUI+OT (ox-alpha, qwen3.8-27b) · OpenCode×grok-4.6 (bare-boot
entry, post-F-005) · Hermes-desktop×grok-4.6 (full story loop, gate held).
Findings: F-004/F-005/F-006; A-001+A-002 confirmed.
Next real project lives in this repo. Alternate sessions deliberately
between Hermes and OWUI+OT instances.
**Cold-start resume test** (the regression suite): fresh chat, zero
history, repo only → agent states project status correctly, picks up the
resume cue, executes one ready story within protocol, updates STATE.md +
decisions.md — with no clarifying questions.
Run per harness × at least two different models (both platforms are
model-agnostic; use that). Every friction → FAILURES.md.

Runbook — each arm is a fresh session with zero context, run against a
current clone of origin (origin must contain the signed ready story):
- **Arm A · OWUI+OT × model 1:** fresh OT workspace → `git clone
  https://github.com/monnkeebrain/keel.git && cd keel` → new chat, no
  system prompt → first message: content of adapters/owui-system-prompt.md
  followed by: `boot, then take the ready story.` (tests adapter entry)
- **Arm B · Hermes × model 2:** clone repo → `hermes` inside it (native
  .hermes.md discovery) → first message: `boot.` No pasted rules allowed —
  this arm tests zero-instruction entry. (requires Hermes installed)
- **Arm C · OWUI+OT × model 2:** repeat Arm A with a different model in
  the dropdown.
PASS per arm: BOOT line matches `bin/boot` output · executes the ready
story via pack within protocol · STATE.md + decisions.md updated · final
report cites evidence (commit hash). Deviations → FAILURES.md immediately.
- Exit gate: resume test green on both harnesses × 2 models.

### P3 — Staff validation
Prerequisites, in order: US-16 overview v2 first (bin/init emits the new
overview structure), then US-15 (`bin/init` clean skeleton) — staff start
from a blank workspace with `src/` for code, not from this repo's history.
Hand the same pattern to staff on one real project (Codex/OpenCode).
Their friction reports are the second failure stream — and the harder
test, because they did not write the method.
- Exit gate: one staff member completes a story end-to-end without
  private instruction from you outside the repo.

### P4 — Story workshop as skill
Package the elicitation protocol as an agentskills.io-compatible skill
(`skills/story-workshop/SKILL.md`): question tree → draft story →
`gate check` → operator/stakeholder sign-off. Loads natively in Hermes
and Open WebUI; degrades gracefully to copy-pack elsewhere.
Pilot: one real non-technical stakeholder.
- Metrics vs your hand-written baseline (25-story Erdt corpus):
  time-to-ready-story, post-accept defect rate.
- Exit gate: assisted stories land within striking distance of baseline;
  gaps feed FAILURES.md as encoding deficits.

### P5 — Refinement, failure-driven only
Governance per AMENDMENTS.md. Resume suite reruns on every amendment.
MCP reconsidered only after months of file+CLI stability and a proven
repetitive operation.

### P6 — Epic protocol (candidate)
Dispatch unit larger than one story. Agent presents the epic; human
`accept E-n` (all remaining review stories + epic) or partial + reassign.
Plan: `docs/epic-protocol.md`. Not in METHOD until a signed story.
MVP is 1H+1A chaining. Store: `data/epics/E-n.json`. WIP: 1, then 3 at E8.
- Exit gate: plan accepted by operator; then E7 stories drafted and signed.

### P7 — Isolation / 1H+N (candidate)
Epic = isolation unit. Worktrees multiply `src/`+`archive/` editors.
Main is the only writer of the store. `bin/wt add|ls|present|accept|reject`.
Join = path-import, not whole-branch merge. Pull of main stays ff-only.
1:1 creates no worktree. Plan: `docs/e8-isolation.md`.
- Exit gate: operator accepts this plan; then E8 stories drafted.

## Standing metrics (only four)

1. Resume-test pass rate per harness × model
2. Token cost per session (spot-check via /usage or equivalent)
3. Time-to-ready-story
4. Post-accept defect rate
