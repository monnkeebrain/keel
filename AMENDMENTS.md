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
| A-001 | 2026-08-25 | B | Reporting law added to P-BOOT: "done" claims must cite commit hash or artifact path; unevidenced reports are void | F-001 | PROPOSED → TRIAL — first positive signal: cold-start agent self-applied evidence discipline unprompted |
| A-002 | 2026-08-25 | B | Split P-BOOT pull semantics: no remote configured → note + continue (local truth canonical); remote configured + pull fails → hard stop. §0 ritual gains "origin optional in solo mode" | F-002 | PROPOSED → TRIAL (next dogfood project) |
