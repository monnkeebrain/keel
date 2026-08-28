# Evidence bundle · US-35 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-35-20260828-124346.md
HEAD at claim: ed66cec · CLAIM US-35

AC-1 epic-accept E98 (listed done+review) → ACCEPT E98, review→done,
    epic→done. PASS

AC-2 epic-accept refuses (exit non-zero) when any listed story is
    in_progress, ready, backlog, or blocked; epic stays review. PASS

AC-3 bin/park US-Tpark → PARK US-Tpark, status blocked. PASS

AC-4 reassign --to backlog removes id from epic, status backlog;
    --to ready keeps id, status ready;
    --to E99 moves id and story.epic. PASS

AC-5 METHOD P-EPIC-ACCEPT names bin/park, bin/reassign, bin/epic-accept
    as operator-only; agent must not run them. PASS

AC-6 adapters (AGENTS.md, .hermes.md, owui-system-prompt.md) after
    bin/adapt: "must not run park, reassign, or epic-accept". PASS

DoD-1 stdlib; wrappers bin/epic-accept, bin/park, bin/reassign. PASS
DoD-2 acceptance PENDING operator.

Fixtures restored. Live US-34 remains review; no leftover E9*.json.
