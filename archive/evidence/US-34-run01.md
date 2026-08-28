# Evidence bundle · US-34 run01 · 2026-08-28
Executor: assistant session · pack: archive/packs/US-34-20260828-124105.md
HEAD at claim: 3ecc13b · CLAIM US-34

AC-1 AMENDMENTS.md A-007 records optional depends_on[] of story ids.
    Status `recorded` (A-004/A-005/A-006 already TRIAL; max 2 in trial). PASS

AC-2 US-35 (no depends_on): bin/gate exit 0, no BLOCKER. PASS

AC-3 US-35 depends_on US-999: GATE FAIL, BLOCKER names id not in store. PASS

AC-4 claim US-35 with depends_on US-34 (in_progress): exit 1,
    `claim refused · US-35 depends_on US-34 is in_progress (need done)`;
    US-35 stayed ready. PASS

AC-5 pack US-35 same dep: exit 1, pack refused. Pack allowed when
    depends_on US-33 (done). PASS

AC-6 epic-pack: US-35 depends_on US-34 → unlocked no;
    US-36 depends_on US-33 → unlocked yes; first story pack US-36. PASS

AC-7 existing stories have no depends_on key; bin/verify exit 0. PASS

DoD-1 stdlib. PASS
DoD-2 METHOD.md §2 names optional depends_on[]. PASS
DoD-3 acceptance PENDING operator.

Fixtures restored; no leftover depends_on keys; no E7.json left.
