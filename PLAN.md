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

### P2 — Two-harness dogfood (the real gate)
Next real project lives in this repo. Alternate sessions deliberately
between Hermes and OWUI+OT instances.
**Cold-start resume test** (the regression suite): fresh chat, zero
history, repo only → agent states project status correctly, picks up the
resume cue, executes one ready story within protocol, updates STATE.md +
decisions.md — with no clarifying questions.
Run per harness × at least two different models (both platforms are
model-agnostic; use that). Every friction → FAILURES.md.
- Exit gate: resume test green on both harnesses × 2 models.

### P3 — Staff validation
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

## Standing metrics (only four)

1. Resume-test pass rate per harness × model
2. Token cost per session (spot-check via /usage or equivalent)
3. Time-to-ready-story
4. Post-accept defect rate
