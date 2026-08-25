# Evidence bundle · US-13 run01 · 2026-08-25
Executor: assistant session (ox-alpha) in keel working repo · pack: archive/packs/US-13-20260825-132036.md (HEAD d4c4798)

AC-1 (checks exist, one line each):
  TEST1 healthy repo: PASS lines for stories.json array / IDs unique /
  decisions headings / adapters match / overview up-to-date (after render).
AC-2 (PASS/WARN/FAIL prefixes): visible in all outputs below.
AC-3 (exit codes): TEST1 exit=0 · TEST2 exit=1.
AC-4 (stale adapters via regen-diff): TEST2 injected 'hand-edit drift' into
  adapters/AGENTS.md -> FAIL "adapters out of sync — run bin/adapt (AGENTS.md)".

Test transcripts:
--- TEST1 healthy (exit 0)
PASS  stories.json parses as array
PASS  story IDs unique
PASS  decisions.md D-nn headings well-formed
PASS  adapters match regeneration
WARN  overview.html older than store — run bin/render   [pre-render state]
VERIFY WARN · 0 fail · 1 warn
--- TEST1b healthy after bin/render (exit 0): all five checks PASS, VERIFY PASS
--- TEST2 broken clone /tmp/keel-broken (exit 1)
PASS  stories.json parses as array
FAIL  story IDs unique — duplicates: ['US-01']
PASS  decisions.md D-nn headings well-formed
FAIL  adapters out of sync — run bin/adapt (AGENTS.md)
WARN  overview.html older than store — run bin/render
VERIFY FAIL · 2 fail · 1 warn

DoD-1 stdlib only: imports = argparse, datetime, json, re, subprocess, sys, pathlib.
DoD-2 METHOD.md §4 documents bin/verify (lines ~124–128).
DoD-3 acceptance via accept-flow with this evidence: PENDING operator.

Transparency note: during implementation the cmd_adapt function header was
accidentally removed by refactor; caught by syntax+smoke test BEFORE any
commit; fixed same turn. Never landed broken.
