# Evidence bundle · US-20 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-20-20260828-093912.md
HEAD at run: fabddbf

AC-1 bin/claim US-24 (ready) → in_progress, prints CLAIM US-24
    stdout:
    CLAIM US-24
    root /Users/joshuahemel/Documents/08_code/keel
    HEAD fabddbf
    status after: in_progress. PASS

AC-2 claim when not ready exits non-zero, stories.json unchanged
    claim US-24 again: exit 1, stderr
    claim refused · US-24 status is in_progress (need ready)
    stories.json bytes identical. Also claim US-19 (done): exit 1. PASS

AC-3 CLAIM stdout includes working-copy root and short HEAD
    root matches git rev-parse --show-toplevel
    HEAD matches git rev-parse --short HEAD (fabddbf). PASS

AC-4 after claim, bin/state Active work line contains the ID
    - Active work: 1 (US-24). PASS

AC-5 after bin/render, overview In flight contains the ID
    In flight table row td.id US-24. PASS

AC-6 two claims: Active work lists both; In flight table two data rows
    claim US-25 → Active work: 2 (US-24, US-25)
    overview rows: US-24 and US-25. PASS
    US-24 and US-25 restored to ready after the fixture.

AC-7 claim does not create a worktree; adds no story fields
    git worktree list unchanged (single main worktree)
    keys after claim == schema v0 keys (no assignee/claimer). PASS

DoD-1 bin/claim wrapper + dispatcher (chmod +x). PASS
DoD-2 stdlib imports: argparse, datetime, json, re, subprocess, sys, pathlib. PASS
DoD-3 METHOD.md P-RUN names claim before pack; multiple in_progress IDs allowed. PASS
DoD-4 AMENDMENTS.md A-004 Tier B TRIAL. PASS
DoD-5 acceptance via accept flow with this evidence: PENDING operator.

US-20 itself claimed to in_progress at end of run, then flipped to review
for operator judgment. Test fixtures US-24/US-25 are ready again.
