# Evidence bundle · US-32 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-32-20260828-115352.md
HEAD at claim: 77db958 · CLAIM US-32

AC-1 epic file keys id, title, goal, stories, depends_on, scope, status
    E7 fixture written with those keys. PASS

AC-2 epic-gate E7 well-formed, stories exist, story.epic=E7 → exit 0 PASS
    EPIC-GATE E7 · Epic protocol MVP / PASS

AC-3 non-zero on missing id / story.epic mismatch / undone depends_on / touch overlap
    US-999 missing; US-18 epic E5 vs E98; E7 not done as depends_on;
    METHOD.md overlaps E7. All FAIL exit 1. PASS

AC-4 claim ready → in_progress, prints CLAIM E-n; WIP 1 refuses second
    CLAIM E7; E98 claim refused · E7 is in_progress. PASS

AC-5 claim not ready: E7 backlog, exit 1, file bytes unchanged. PASS

AC-6 wipe_epic_files: json gone, directory remains (init calls this). PASS

AC-7 verify FAIL duplicate epic ids ['E7'] exit 1; after cleanup unique. PASS

DoD-1 wrappers bin/epic-gate, bin/epic-claim, stdlib. PASS
DoD-2 METHOD.md §2 names data/epics/*.json. PASS
DoD-3 acceptance PENDING operator.

Fixtures removed. data/epics/.gitkeep only.
