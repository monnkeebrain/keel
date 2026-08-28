# Evidence bundle · US-22 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-22-20260828-101749.md
HEAD at claim: 429487b · CLAIM US-22

AC-1 AMENDMENTS.md records optional scope.touch / scope.forbid
    A-005 2026-08-28 Tier B TRIAL. PASS

AC-2 METHOD.md states scope prefixes are relative to the working-copy root
    §2: optional scope.touch[] / scope.forbid[]; Scope prefixes are
    relative to the working-copy root. PASS

AC-3 story without scope: bin/gate exit 0, no BLOCKER
    US-18 (backlog, no scope): exit 0, PASS WITH NOTES, no BLOCKER. PASS

AC-4 gate MINOR when backlog/ready and scope missing or both arrays empty
    US-18: MINOR scope unset …
    US-19 (done): no scope MINOR. PASS

AC-5 pack lists each touch and forbid prefix when present
    Temporary scope on US-18 {touch: [src/app/], forbid: [bin/]}:
    - touch src/app/
    - forbid bin/
    Restored; US-18 has no scope key. PASS

AC-6 pack prints 'Scope: unset' when field absent
    bin/pack US-22 contains ## Scope and Scope: unset. PASS

AC-7 bin/verify exits 0 while existing stories have no scope key
    extra fields none; verify exit 0. PASS

DoD-1 stdlib only (no new imports). PASS
DoD-2 METHOD.md §2 schema line names optional scope. PASS
DoD-3 acceptance via accept flow with this evidence: PENDING operator.
