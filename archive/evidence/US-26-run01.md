# Evidence bundle · US-26 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-26-20260828-105418.md
HEAD at claim: 06d4139 · CLAIM US-26

AC-1 VERSION is the single line 0.1.1
    repr '0.1.1\\n'. PASS

AC-2 METHOD.md Status line contains 0.1.1
    Status: v0.1.1 — protocols defined, toolchain live. PASS

AC-3 bin/boot stdout contains keel v0.1.1
    BOOT ok · keel v0.1.1 · … PASS

AC-4 bin/verify exits 0
    VERIFY WARN then PASS after render · 0 fail. PASS

AC-5 README disclaimer above Status
    **Disclaimer:** Alpha. Work in progress. Use at your own risk.
    appears before **Status:**. PASS

AC-6 Changelog lists US-19..US-25 by id and one-line title. PASS

AC-7 Changelog names F-007 (bin/state KeyError on missing blocked bucket)
    and D-014. PASS

DoD-1 no new bin/ command (scope forbid bin/). PASS
DoD-2 acceptance via accept flow with this evidence: PENDING operator.
