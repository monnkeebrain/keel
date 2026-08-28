# AMENDMENTS

How METHOD.md may change. Two goals in tension: the method must evolve
with evidence (failures are real), and it must not decay through
accretion of patches, over-fitting to single bad days, or rules that
outlive their reason. The mechanisms below bias every default toward
simplicity.

## Tier model

- **Tier A — Invariants** (METHOD §1). The load-bearing laws.
  Changing one requires: written rationale + resume suite green on ALL
  primary harnesses × models + explicit operator decision recorded here.
  Expect a handful of Tier A changes per year, not per month.
- **Tier B — Protocols** (METHOD §3+). Amendable via lifecycle below.
  Deletions and simplifications get the fast path.

## Amendment lifecycle

Every change to Tier B is an entry here with status:

    PROPOSED → TRIAL → CONFIRMED   |   REVERTED

1. **PROPOSED** — cites a FAILURES entry (`protocol_bug`/`ambiguity` only)
   or an explicit simplification. States hypothesis: what this improves.
2. **TRIAL** — runs on exactly one real project. Not stackable:
   max 2 amendments in trial at once, so effects stay attributable.
3. **CONFIRMED** — only if the resume suite still passes and the failure
   class did not recur on the trial project.
4. **REVERTED** — default outcome for unproven changes:

Sunset rules (the anti-decay teeth):
- An amendment not in TRIAL within 30 days → auto-REVERTED.
- An amendment not CONFIRMED after two projects → auto-REVERTED.
- Burden of proof is always on additions. Removal needs only this ledger
  note: "unused/unjustified after N projects" — no trial required.

## Anti-overfit rule

If a proposed amendment addresses exactly one occurrence of one failure,
log it as `noted` instead. Methods are fitted to patterns, not incidents.

## Ledger

| ID | Date | Tier | Change summary | Cites | Status |
|----|------|------|----------------|-------|--------|
| A-001 | 2026-08-25 | B | Reporting law added to P-BOOT: "done" claims must cite commit hash or artifact path; unevidenced reports are void | F-001 | CONFIRMED 2026-08-25 — evidence citation observed in every matrix arm (ox-alpha, grok-4.6 ×2 paths, qwen3.8); Hermes halted at acceptance gate unprompted; no F-001-class recurrence |
| A-002 | 2026-08-25 | B | Split P-BOOT pull semantics: no origin → note "origin: none — local mode" + continue; origin configured + pull fails → hard stop. Local-only first-class; hosted standard = private GitHub/GitLab (push/pull only) | F-002 | CONFIRMED 2026-08-25 — strict branch exercised cleanly (Arms A/C pull + staleness check); local-only branch remains staff default |
| A-003 | 2026-08-25 | B | P-END gains mandatory `bin/verify` before push | F-005 | CONFIRMED 2026-08-25 — every push since F-003 preceded by verify green; duplicate-subparser crash caught by battery pre-push (US-15/17 runs) |
| A-004 | 2026-08-28 | B | P-RUN gains `bin/claim` before pack; multiple in_progress IDs allowed | simplification (in_progress was displayed but never written) | TRIAL |
| A-005 | 2026-08-28 | B | Schema v0 optional `scope.touch` / `scope.forbid` path-prefix arrays; prefixes relative to working-copy root | US-22 | TRIAL |
| A-006 | 2026-08-28 | B | P-EPIC-STORY/SIGN/RUN/PRESENT/ACCEPT: take E-n chains unlocked stories; park/reassign operator-only; accept E-n only when listed stories are done | US-31 | TRIAL |
