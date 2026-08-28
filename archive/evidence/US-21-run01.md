# Evidence bundle · US-21 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-21-20260828-094513.md
HEAD at claim: 6dd6195 · CLAIM US-21

AC-1 pack Write-set heading lists src/, archive/evidence/, archive/packs/
    ## Write-set
    - src/
    - archive/evidence/
    - archive/packs/
    PASS

AC-2 paths relative to this working-copy root
    pack line: Paths are relative to this working-copy root. PASS

AC-3 status may be set to in_progress, review, or blocked
    pack line: This story's status may be set to in_progress, review, or blocked. PASS

AC-4 status done is forbidden
    pack line: Status done is forbidden. PASS

AC-5 Do-not-write heading lists METHOD.md and bin/
    ## Do-not-write
    - METHOD.md
    - bin/
    PASS

AC-6 METHOD.md P-RUN names the same write-set
    Default write-set paragraph under P-RUN lists src/, archive/evidence/,
    archive/packs/, working-copy root, status triad, done forbidden,
    METHOD.md, bin/. PASS

AC-7 bin/adapt then bin/verify exits 0
    adapt wrote AGENTS.md / .hermes.md / adapters/owui-system-prompt.md
    verify exit 0 (WARN overview stale before final render). PASS

DoD-1 stdlib: argparse, datetime, json, re, subprocess, sys, pathlib. PASS
DoD-2 existing stories: extra fields {} (schema v0 only). PASS
DoD-3 acceptance via accept flow with this evidence: PENDING operator.
