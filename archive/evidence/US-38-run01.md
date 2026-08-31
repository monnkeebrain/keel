# Evidence bundle · US-38 run01 · 2026-08-31
Executor: assistant session · pack: archive/packs/US-38-20260831-131357.md
HEAD at claim: ebd037f · CLAIM US-38

EXCEPTION (operator-approved 2026-08-31): packed scope.forbid was
bin/, but AC-3 requires adapter text that only `bin/keel` `_core_text`
emits. Operator expanded scope to METHOD.md + bin/keel. No new
command. Not a piggyback of US-39.

AC-1 METHOD: cwd provided by operator or harness. PASS
AC-2 METHOD: agent does not create a git worktree as a KEEL verb. PASS
AC-3 bin/adapt: AGENTS.md, .hermes.md, owui-system-prompt.md all
    contain "must not run git worktree add". PASS
AC-4 bin/verify exits 0. PASS
AC-5 no bin/review, no bin/wt. PASS

DoD-1 no new bin/ command (only bin/keel _core_text). PASS
DoD-2 acceptance PENDING operator.
