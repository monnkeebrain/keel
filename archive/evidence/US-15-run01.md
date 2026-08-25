# Evidence bundle · US-15 run01 · 2026-08-25
Executor: assistant session · pack: archive/packs/US-15-20260825-192319.md

AC-1 resets: init on throwaway copy -> stories.json == [] · decisions.md
    header+format only (no D-001). PASS
AC-2 ledgers keep rules, empty ledger: FAILURES keeps '## Rules' + '(none yet)',
    no F-entries; AMENDMENTS keeps 'Sunset rules' + '(none yet)', no A-rows. PASS
AC-3 archive cleared keeping structure: packs/ and evidence/ exist and are
    empty; stray files removed. PASS
AC-4 src/.gitkeep created. PASS
AC-5 method files byte-identical: diff -rq pristine-vs-init (excluding
    data/ decisions STATE FAILURES AMENDMENTS archive overview src .git)
    -> IDENTICAL. PASS
AC-6 STATE regenerated: phase 'P0 — project start', cue references
    story-workshop, last verified never. PASS
AC-7 guard: second init without --force -> exit 1, message names --force;
    third run with --force -> exit 0. PASS

DoD-1 stdlib only (argparse/json/re/subprocess/sys/pathlib/datetime/shutil).
DoD-2 METHOD §4 documents bin/init (wrapper list + usage paragraph).
DoD-3 acceptance via accept flow with this evidence: PENDING operator.

Transparency note: initial impl commit registered a duplicate argparse
subparser ('verify'), crashing every command; caught immediately by the
test battery before push/review; deduplicated same turn (A-003's
verify-before-push rule would have gated this at P-END).
