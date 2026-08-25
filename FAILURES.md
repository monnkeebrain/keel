# FAILURES

Every failure observed while using the method gets one entry here.
This log is the only legitimate input for changing METHOD.md
(see AMENDMENTS.md) — but not every failure class justifies a change.

## Entry format

    ## F-<n> · <date> · <harness/model>
    - Class: protocol_bug | ambiguity | environment | human_error | scope
    - What happened: (facts, one paragraph max)
    - What the method should have prevented: (if anything)
    - Disposition: noted | clarification | amendment-proposed F-<n>

## Rules

1. Log failures immediately, in fact form. No interpretation at log time.
2. Only `protocol_bug` and `ambiguity` classes can drive METHOD changes.
   `environment` → harness note; `human_error`/`scope` → training signal,
   not method change.
3. An unlogged failure cannot justify an amendment. Ever.

## Ledger

## F-001 · 2026-08-25 · assistant session, local repo
- Class: protocol_bug
- What happened: Assistant reported P-BOOT/P-END definitions, decisions.md
  creation, and a commit as completed. None of it existed in the repo.
  The false report was only exposed by reading files in the next turn.
  Operator had accepted the report at face value.
- What the method should have prevented: P-END's evidence rule did not
  exist yet, so session reports carried no obligation to cite artifacts
  or commit hashes.
- Disposition: amendment-proposed → A-001 ("reporting law": done claims
  must cite evidence; unevidenced reports are void).

## F-002 · 2026-08-25 · OWUI+OT cold-start test (no system prompt, no tools, no remote)
- Class: ambiguity
- What happened: P-BOOT step 2 mandates "pull fails → stop and report".
  In a remote-less local repo `git pull --ff-only` always fails (no
  tracking information). Test agent reported the failure loudly,
  justified continuing (clean tree, local HEAD canonical), completed
  reads, emitted the BOOT line, and mutated nothing. Correct judgment —
  but the protocol did not authorize that path.
- What the method should have prevented: conflation of two failure
  classes: "no remote configured" (benign, local-first mode) vs "remote
  configured but pull failed" (dangerous, stale-truth risk).
- Disposition: amendment-proposed → A-002 (split pull semantics).

## F-003 · 2026-08-25 · bin/state writer vs parser format drift
- Class: ambiguity (tool-level; caught by bin/boot spot-check)
- What happened: rewritten bin/state emitted "Resume cue:" without the
  leading dash the STATE parser requires; every regenerated STATE.md lost
  its cue to boot/render readers (rendered as "-"). Discovered because a
  final BOOT-line check was run instead of assuming success.
- What the method should have prevented: nothing more needed — verify/boot
  spot-checks caught it pre-push. Logged for the pattern: writers and
  readers of an artifact must be changed in the same commit.
- Disposition: noted + fixed same commit (writer emits bullet form).
  No METHOD change required.

## F-004 · 2026-08-25 · Hermes desktop / grok-4.6
- Class: environment
- What happened: Operator first message was `boot`. Agent searched prior
  sessions, ran `hermes doctor`, reported machine status, and did not emit
  a BOOT line. BOOT emitted only after the operator said they were testing
  the keel folder. Repo contained `adapters/.hermes.md` and
  `adapters/AGENTS.md`; no `AGENTS.md` or `.hermes.md` at repo root. Hermes
  desktop did not inject the adapter into the session prompt.
- What the method should have prevented: PLAN P2 Arm B claims
  zero-instruction Hermes entry via native `.hermes.md` discovery.
  Generated adapter path and harness discovery path did not meet.
- Disposition: noted
