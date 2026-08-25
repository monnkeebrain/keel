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
