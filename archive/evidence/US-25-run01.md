# Evidence bundle · US-25 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-25-20260828-103244.md
HEAD at claim: 0d93ff0 · CLAIM US-25

AC-1 METHOD.md §2: concurrent writes to data/stories.json inside one
    working copy are unsupported. PASS

AC-2 METHOD.md: parallel agents isolate via git worktrees or separate
    clones, then git pull --ff-only and git push. PASS

AC-3 METHOD.md: creating a worktree is not required for 1:1 sessions. PASS

AC-4 docs/tips-and-tricks.md "One writer per working copy" states both
    the one-writer rule and the worktree isolation path. PASS

AC-5 this story did not create a worktree, bin/worktree, data/stories/,
    or split stories.json. git worktree list: single main copy. PASS

AC-6 bin/adapt then bin/verify exits 0. PASS

DoD-1 no schema change (no new story fields). PASS
DoD-2 acceptance via accept flow with this evidence: PENDING operator.
