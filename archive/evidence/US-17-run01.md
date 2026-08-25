# Evidence bundle · US-17 run01 · 2026-08-25
Executor: assistant session · pack: archive/packs/US-17-20260825-195142.md

AC-1 git bootstrap: ZIP-style export (no .git) -> init creates repo on
    branch main, prints local-only mode; git status/branch work after. PASS
AC-2 post-init git status lists workspace files as untracked (observed
    .gitignore/.hermes.md/AGENTS.md + others on branch main). PASS
AC-3 existing-.git scenario unchanged: clone copy init exits 0, no re-init
    call, prior behavior preserved. PASS
AC-4 double-run guard: second run exits 1 naming --force. PASS
AC-5 PLAN.md reset to blank project stub (no keel phases). PASS
AC-6 README replaced by instance edition (usage kept, status/history gone;
    upstream README separately documents ZIP/template paths per DoD-3). PASS
AC-7 purity sweep widened to whole workspace (all text files, skip .git):
    zero keel-build identifier matches post-scrub. Sweep itself caught six
    residual references (SKILL header, four METHOD annotations, two example
    IDs) which were generalized upstream before this run. PASS

DoD-1 subprocess git helper pattern, stdlib only. PASS
DoD-2 template texts embedded as constants (TEMPLATE_PLAN/TEMPLATE_README)
    in one place; upstream pulls ship improvements. PASS
DoD-3 README documents ZIP path + Use-this-template alongside clone. PASS
DoD-4 acceptance via accept flow with this evidence: PENDING operator.

Process notes: two test-harness lessons logged inline during the run —
(1) single-line grep cannot verify multi-line HTML blocks (use targeted
string search); (2) git archive exports committed state only, so working-
tree scrubs must be committed before export-based tests.
