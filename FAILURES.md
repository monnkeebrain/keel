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

(none yet — first entries expected during P2)
