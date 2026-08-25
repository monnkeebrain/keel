# Evidence bundle · US-14 run01 · 2026-08-25
Executor: Hermes desktop · grok-4.6 / xAI · pack: archive/packs/US-14-20260825-175040.md (HEAD 51d7f33)

AC-1 (three sections): laws/stack.md exists. Headings at lines 5/11/16:
  ## Toolchain · ## Artifacts · ## Runtime
AC-2 (Toolchain): lines 7–9 state stdlib Python only for bin/, no pip installs, no build step.
AC-3 (Artifacts): lines 13–14 state markdown/JSON files in git, no external database.
AC-4 (Runtime): line 18 names git as the only runtime dependency.
AC-5 (pack lists law/stack.md under Laws):
  bin/pack US-14 → `## Laws` then `### law/stack.md`
  bin/pack US-13 → line 23 `## Laws` · line 25 `### law/stack.md`
  bin/pack US-01 → line 25 `## Laws` · line 27 `### law/stack.md`

DoD-1: wc -l laws/stack.md → 18 (≤ 40).
DoD-2: operator accepted 2026-08-25 this session. Accept commit cites this file.

Also run: bin/render → overview.html 16554 bytes.
bin/verify → PASS · 0 fail · 0 warn (exit 0).
No bin/pack code change: existing glob of laws/*.md already emits `### law/<name>`.
