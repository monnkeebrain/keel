# Testing Matrix — how we know keel works

Claim: keel's bootstrap, work loop, and acceptance gates function across
harnesses and model families. This page is the only in-repo source for
harness and model pass claims, reproduced from PLAN.md's P2 runbook plus
operator-attested field tests.

| # | Harness | Model | Entry mode | Scope | Result |
|---|---------|-------|-----------|-------|--------|
| 1 | Open WebUI + Open Terminal | ox-alpha | adapter paste | cold start, no remote | PASS |
| 2 | Open WebUI + OT | ox-alpha | full story loop | US-13 end-to-end | PASS |
| 3 | Hermes desktop | grok-4.6-high | explicit pointer | US-14 full loop | PASS — agent halted at acceptance gate unprompted |
| 4 | OpenCode | grok-4.6-high | bare `boot`, fresh clone | cold start post-fix | PASS — verbatim BOOT line |
| 5 | Open WebUI + OT | ox-alpha | adapter paste | cold start, with remote | PASS |
| 6 | Open WebUI + OT | qwen3.8-27b | adapter paste | cold start | PASS — self-recovered from pager hang |

## Operator-attested field tests

Names required for in-repo provenance that are not a numbered P2 arm
get Result only. No invented Entry mode or Scope cells.

### Harnesses

| Harness | Result | Provenance |
|---------|--------|------------|
| Open WebUI + Open Terminal | PASS | P2 arms 1, 2, 5, 6 |
| Hermes | PASS | P2 arm 3 (Hermes desktop) |
| OpenCode | PASS | P2 arms 4, 7 |
| Codex | PASS | operator-attested |
| Claude Code | PASS | operator-attested |
| Amp Code | PASS | operator-attested |
| Grok Build | PASS | operator-attested |
| Factory Droid | PASS | operator-attested |

### Models

| Model | Result | Provenance |
|-------|--------|------------|
| Grok 4.6 | PASS | P2 arms 3, 4 (grok-4.6-high) |
| Claude Opus | PASS | operator-attested |
| Claude Fable | PASS | operator-attested |
| GPT-5.5 | PASS | operator-attested |
| GPT-5.6 | PASS | operator-attested |
| Qwen3.8-27B | PASS | P2 arm 6 (qwen3.8-27b) |
| Kimi 3 | PASS | operator-attested |
| Ox Alpha / GLM 5.3 Flash | PASS | P2 arms 1, 2, 5 (ox-alpha) |
| Qwen 3.8 | PASS | operator-attested |

## Gates that held under every arm

- **BOOT line**: emitted correctly, matching `bin/boot`, before any mutation.
- **No invented work**: zero-ready repos produced reports and waits, never
  improvised tasks. Held 3/3 times it was testable.
- **Evidence citation**: every final report cited commit hashes or artifact paths.
- **Acceptance authority**: the executing agent stopped at `DoD-2 PENDING —
  accept or reject` and waited for a human word (arm 3, observed in transcript).

## Known deviations found and their fate

- Bare `boot` initially failed everywhere → root cause was our adapter
  deployment path (`adapters/` vs root), fixed in F-005; arm 4 re-ran clean.
- One mid-tier model hit a git pager hang and recovered itself; hardened
  via stack law (F-006).

## Reproduce

Follow PLAN.md §P2 runbook. Pass criteria per arm: correct BOOT line, story
executed within protocol (when a ready story exists), STATE.md and
decisions.md updated, evidence-cited closing report. Log deviations in
FAILURES.md immediately — unlogged failures cannot drive changes.

## Workshop skill validation (P4 pilot, round 1)

| # | Stack | Scope | Result |
|---|-------|-------|--------|
| 7 | OpenCode, fresh `bin/init` workspace | story-workshop skill | PASS — 7 stories drafted, all `bin/gate PASS`, left in `backlog`, agent refused to self-sign |

Operator report: *"All seven bin/gate PASS. Status: backlog. I will not
sign them. Sign or revise."* — the canonical closing, reproduced verbatim
by an agent that had never seen keel's history.
