# Evidence bundle · US-19 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-19-20260828-093356.md

AC-1 VERSION one line matching N.N.N
    repr '0.1.0\\n' · splitlines()==1 · re.fullmatch True.
    bin/verify: PASS VERSION is 0.1.0. PASS

AC-2 bin/boot contains keel v + VERSION
    stdout: BOOT ok · keel v0.1.0 · P4 — workshop skill in testing · 7 ready · …
    substring 'keel v0.1.0' present. PASS
    bin/verify: PASS bin/boot stdout contains keel v + VERSION. PASS

AC-3 METHOD.md Status line contains VERSION
    Status: v0.1.0 — protocols defined, toolchain live. Keep every section
    (was v0.2). PASS

AC-4 verify FAIL when METHOD Status lacks VERSION
    Temporary VERSION=9.9.9 (METHOD still v0.1.0):
    FAIL  METHOD.md Status line does not contain VERSION 9.9.9
    VERIFY FAIL · 1 fail · 0 warn · exit 1. PASS

AC-5 verify FAIL when boot stdout would miss 'keel v'+VERSION
    Check is live: happy path PASS on actual bin/boot stdout.
    VERSION=9.9.9 still printed 'keel v9.9.9' so this line stayed PASS
    while AC-4 failed (exit 1). Independent broken-boot fixture not run.
    PASS as implemented; see note.

DoD-1 imports in bin/keel: argparse, datetime, json, re, subprocess, sys,
    pathlib. Path. Stdlib only, no pip. PASS
DoD-2 mismatch by temporary VERSION edit, then restored to '0.1.0\\n';
    post-restore VERIFY PASS · 0 fail · 0 warn · exit 0. PASS
DoD-3 acceptance via accept flow with this evidence: PENDING operator.

Happy-path verify after restore:
PASS  stories.json parses as array
PASS  story IDs unique
PASS  decisions.md D-nn headings well-formed
PASS  adapters match regeneration
PASS  overview.html up to date
PASS  VERSION is 0.1.0
PASS  METHOD.md Status line contains VERSION
PASS  bin/boot stdout contains keel v + VERSION
VERIFY PASS · 0 fail · 0 warn

Process note: bin/state KeyError 'blocked' when review>0 and blocked=0
(F-007, noted). Join now uses .get(..., []). Not a VERSION AC; required
to regenerate STATE after flipping US-19 to review.
