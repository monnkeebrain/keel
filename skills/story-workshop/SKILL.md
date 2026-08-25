---
name: story-workshop
description: >
  Turn a stakeholder's messy intent into a gated, signable keel story with
  acceptance criteria and definition of done. Use whenever someone describes
  a problem, wish, or change that must become a user story — especially when
  the stakeholder struggles to express themselves precisely.
---

# Story Workshop

Elicit, don't invent. The stakeholder owns the intent; you own the structure.
Your job is to ask questions that surface what they already know, then shape
it into a contract that survives `bin/gate`. Never fabricate requirements the
stakeholder did not confirm.

## The question tree (ask, listen, write)

Work through five rounds. Skip a round only if prior answers already cover it.
Quote the stakeholder's own words back when drafting — their vocabulary is the
contract's vocabulary.

### R1 · Problem (what hurts today)
- Walk me through what happens today, step by step, when this problem occurs.
- Who does what, and where does it slow down or break?
- What does it cost — time, money, errors, frustration?

### R2 · Outcome (what changes tomorrow)
- After this is solved, what will one specific person do differently?
- What will they see on screen / in the report / in the file?
- How will we know within a week that it worked?

### R3 · Scope (what stays out)
- What is explicitly NOT part of this?
- What existing behavior must remain untouched?
- If we could only do half, which half still helps?

### R4 · Verification (how we'll prove it)
- If you tested this tomorrow morning, what exactly would you click/open/run?
- What number, count, or state tells you it is correct?
- What result would make you say "no, that's not it"?

### R5 · Edges (where reality gets messy)
- When does this NOT apply? What about empty inputs, weekends, new users?
- What should happen when something fails mid-way?
- Who or what is excluded?

If R4 stays unanswered after genuine effort: the story is not ready. Park it
with the open questions recorded. An unverifiable story is a wish, not a contract.

## Quality bar (enforced — these kill stories at the gate)

Ban these words in AC/DoD: works well, intuitive, robust, nice, better,
fast, clean, modern, user-friendly, properly. Replace each with an
observation a person can make or a check a command can run.

- One outcome per story. Split anything joined by "and" unless both halves
  are trivially small.
- Every AC traces to something the stakeholder said in R1–R5.
- Prefer "X shows Y" over "system handles X". Concrete counts beat adjectives.
- No implementation micromanagement unless implementation IS the contract.
- Errors and empty states are AC material, not footnotes.

## Construction

1. Narrative triad: As a <R1 actor>, I want <R2 capability>, so that <R1/R2 value>.
2. AC list: 1–7 observable checks, each traceable to an answer, each with a
   verification hint (what to open, run, or measure).
3. DoD list: cross-cutting bar only (laws obeyed, docs touched, evidence planned).
4. Scope: in/out bullets straight from R3.
5. Save as story entry (status backlog), then run `bin/gate <ID>`.
6. Fix blockers WITH the stakeholder — never silently rewrite their intent.

## Closing (the US-14 pattern)

Present a compact contract table: ID, title, triad, AC count, DoD count,
scope summary. Then stop and say: **"Sign or revise."**

The human signs (commit `sign US-xx`) or requests changes. You never promote
your own draft to ready, and you never accept completed work — acceptance
always belongs to the human.

Worked case study: US-13/US-14 in this repository's history — including the
canonical ending where the executing agent reported `DoD-2 PENDING` and
waited for "Accept or reject."
