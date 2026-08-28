# DECISIONS

Newest first. Format: `## D-nn · YYYY-MM-DD · title` followed by
Rationale / Consequences / Refs. Corrections via supersede, never edits.

## D-026 · 2026-08-28 · v0.1.2 cut: E7 changelog, README current
Rationale: clones still saw 0.1.1 and a README that said entering P3
and a BOOT line without epics after E7 had landed.
Consequences: VERSION is 0.1.2. METHOD Status matches. README changelog
lists US-31..36. Loop BOOT line includes `epics <ids or none>`. Status
paragraph no longer says entering P3. bin/ untouched.
Refs: US-37; archive/evidence/US-37-run01.md.

## D-025 · 2026-08-28 · E7 complete; E8 isolation planned next
Rationale: US-31..36 accepted. Dispatch works for 1H×1A×1 working
copy. Two agents still cannot both land claims on stories.json.
Consequences: candidate `docs/e8-isolation.md`. Abandoned US-27..30
stay abandoned; E8 gets new ids. VERSION 0.1.2 is a signed story
(US-37), not residue. Not METHOD until E8 stories are signed and taken.
Refs: docs/e8-isolation.md; D-016; US-36.

## D-024 · 2026-08-28 · overview groups stories by epic
Rationale: a flat story list is unusable once work is dispatched as
epics. The operator judges a presented epic as a cluster.
Consequences: `bin/render` writes one heading per epic id found on
stories. Same-epic stories sit only under that heading. Missing epic
field → heading `ungrouped`. Needs-you still lists review/blocked
stories and, when `data/epics` exists, review epics. Still one file,
no script.
Refs: US-36; archive/evidence/US-36-run01.md.

## D-023 · 2026-08-28 · epic-accept, park, reassign are operator commands
Rationale: agents must not park, close, or move work off an epic.
Those keys stay human.
Consequences: `bin/epic-accept E-n` batch-dones remaining review
stories and the epic when every listed id is review or done; refuses
in_progress/ready/backlog/blocked. `bin/park` → blocked.
`bin/reassign --to backlog|ready|E-n`. Adapters forbid agents from
running park, reassign, or epic-accept.
Refs: US-35; archive/evidence/US-35-run01.md.

## D-022 · 2026-08-28 · optional story depends_on; claim/pack refuse unless done
Rationale: chaining inside an epic must not build on unaccepted work.
`review` is not `done`.
Consequences: schema v0 optional `depends_on[]` of story ids. Gate
BLOCKER if an id is missing from the store. `bin/claim` and `bin/pack`
refuse unless every predecessor is `done`. epic-pack unlocked = ready
and every depends_on is done. A-007 recorded (not stacked as TRIAL).
Refs: US-34; A-007; archive/evidence/US-34-run01.md.

## D-021 · 2026-08-28 · epic-pack, epic-present, boot lists active epics
Rationale: take E-n needs a cover sheet and a present command, and a
cold BOOT must name in-flight epics. Pack still listed blocked as an
agent-writable status, which contradicted operator-only park.
Consequences: `bin/epic-pack` prints the epic cover then the first
unlocked story pack. `bin/epic-present` sets the epic to review.
`bin/boot` emits `epics <ids or none>`. STATE has Active epics.
Pack write-set: blocked is operator-only. After review, pack the next
unlocked story in this epic without a new take.
Refs: US-33; archive/evidence/US-33-run01.md.

## D-020 · 2026-08-28 · data/epics/*.json + epic-gate + epic-claim
Rationale: one JSON array cannot isolate two epic claims. Gate must
reject missing stories, epic-field mismatch, undone depends_on epics,
and touch overlap. Claim enforces WIP 1.
Consequences: store is one file per epic; bin/init wipes *.json;
bin/verify fails on duplicate epic ids.
Refs: US-32; archive/evidence/US-32-run01.md.

## D-019 · 2026-08-28 · P-EPIC protocols in METHOD (text; no bin yet)
Rationale: take E-n had to be defined before any epic command existed,
or agents would invent machinery.
Consequences: METHOD §3 P-EPIC-STORY/SIGN/RUN/PRESENT/ACCEPT bind;
park/reassign operator-only; accept E-n only when listed stories are
done; take US-xx still does not chain. Store and bin/ are later E7
stories. A-006 TRIAL.
Refs: US-31; A-006; archive/evidence/US-31-run01.md.

## D-018 · 2026-08-28 · Plan accepted: operator-only park/reassign; overview groups by epic
Rationale: Operator accepted the epic plan iff park and reassign require
a human speech act, and overview.html clusters stories by epic.
Consequences: agent may recommend park, must not park. E7 stories
US-31..36 drafted. Not METHOD until those stories are signed and taken.
Refs: docs/epic-protocol.md; D-017.

## D-017 · 2026-08-28 · Epic protocol answers: per-file epics, present, accept E-n
Rationale: Operator closed the five plan questions and the accept
model: take epic → finish/validate unlocked stories → present;
human accepts all (stories+epic) or some + reassign; epic accept
only when every listed story is done.
Consequences: candidate `docs/epic-protocol.md` uses `data/epics/E-n.json`,
chain-after-review, BOOT lists active epics, WIP 1 then 3 at E8, park
when not logical. Not METHOD yet.
Refs: docs/epic-protocol.md; D-016.

## D-016 · 2026-08-28 · US-27..30 abandoned; epic protocol planned first
Rationale: isolation stories (worktree, claimer, story-per-file, clerk)
were unsigned. Operator dropped them so the epic loop could be designed
before more 1:1 machinery. IDs stay (invariant 6).
Consequences: US-27..30 status `abandoned`. Candidate protocol lives in
`docs/epic-protocol.md` (not METHOD). Isolation returns as E8 after E7
MVP is dogfooded. US-18 remains backlog.
Refs: US-27..30; docs/epic-protocol.md.

## D-015 · 2026-08-28 · v0.1.1 cut: alpha disclaimer and changelog
Rationale: clones were reading "method core proven" with no version
match to a changelog and no risk label.
Consequences: VERSION is 0.1.1; METHOD Status and bin/boot agree;
README states alpha / work in progress / use at own risk above Status;
Changelog lists US-19..25 plus F-007 · D-014.
Refs: US-26; archive/evidence/US-26-run01.md.

## D-014 · 2026-08-28 · bin/state must tolerate empty status buckets
Rationale: F-007 — first review-without-blocked story crashed `bin/state`
on `ids_by['blocked']`. The fix shipped inside the US-19 review commit
instead of as its own contract.
Consequences: `cmd_state` joins review/blocked with `.get(key, [])`.
A mid-run tool crash is FAILURES immediately; a fix outside the packed
ACs is a follow-up story, not a passenger. No METHOD change (noted).
Refs: F-007; US-19; e7e35ba.

## D-013 · 2026-08-28 · One writer per working copy; worktrees named
Rationale: stories.json cannot take two writers in one working copy.
Forbidding all parallelism would paint the method into a corner.
Consequences: concurrent writes to `data/stories.json` inside one
working copy are unsupported. Later parallel agents isolate via git
worktrees or separate clones, then `git pull --ff-only` / `git push`.
1:1 sessions do not create a worktree. No file split in this story.
Refs: US-25; archive/evidence/US-25-run01.md.

## D-012 · 2026-08-28 · Matrix is the only in-repo pass-claim source
Rationale: README lagged the tests that had already run. Provenance
must match the ledger, not a stale subset.
Consequences: `docs/testing-matrix.md` names every tested harness and
model (P2 arms plus operator-attested PASS rows). README Status names
nothing absent from that page. The marketing site is out of this repo.
Refs: US-24; archive/evidence/US-24-run01.md.

## D-011 · 2026-08-28 · Overview embeds image evidence
Rationale: UI accept cannot be judged from three quoted markdown lines.
Consequences: Needs-you shows the newest `archive/evidence/US-xx-*.png|.jpg|.webp`
as an img with a relative src, next to the markdown quote when present.
Md-only review stories emit no img. overview.html stays one file, no script.
Refs: US-23; archive/evidence/US-23-run01.md; archive/evidence/US-23-run01.png.

## D-010 · 2026-08-28 · Optional scope.touch / scope.forbid
Rationale: pack said stay in scope but stories had no paths. Parallel
agents (later worktrees) need isolatable contracts.
Consequences: optional `scope` with `touch[]` and `forbid[]` path
prefixes relative to the working-copy root. Missing scope is not a
BLOCKER; gate MINOR on backlog/ready. Pack prints Scope: unset or the
prefixes. A-005 is TRIAL.
Refs: US-22; A-005; archive/evidence/US-22-run01.md.

## D-009 · 2026-08-28 · Pack names the default write-set
Rationale: "do not edit files outside your scope" was a sentence with no
paths. Agents treated METHOD.md and bin/ as fair game.
Consequences: every pack lists Write-set (src/, archive/evidence/,
archive/packs/, relative to this working-copy root) and Do-not-write
(METHOD.md, bin/). Status may move to in_progress/review/blocked; done
stays forbidden. METHOD P-RUN carries the same list.
Refs: US-21; archive/evidence/US-21-run01.md.

## D-008 · 2026-08-28 · bin/claim writes in_progress
Rationale: in_progress was shown on STATE and overview but nothing in
bin/ wrote it. Flight was a ghost.
Consequences: `bin/claim US-xx` is the ready → in_progress lock; it
prints working-copy root and short HEAD so a later worktree protocol
can bind without a schema change. Multiple in_progress IDs are allowed.
A-004 is TRIAL.
Refs: US-20; A-004; archive/evidence/US-20-run01.md.

## D-007 · 2026-08-28 · VERSION file is the only version string
Rationale: METHOD said v0.2 while VERSION and bin/boot said 0.1.0.
Two version strings in one clone is a false report.
Consequences: METHOD Status line carries the VERSION file text;
bin/verify fails when Status or bin/boot drift from VERSION.
Refs: US-19; archive/evidence/US-19-run01.md.

## D-006 · 2026-08-25 · Template purity enforced mechanically; three onboarding ramps
Rationale: staff arrive however they arrive (clone, Use-this-template,
downloaded ZIP). The workspace they land in must contain the full method
and zero keel-build information — intent, not discipline, decides quality.
Consequences: bin/init bootstraps git when missing (local-only mode),
writes instance-edition PLAN/README from embedded constants, and fails
with a listed violation if any build identifier (US/D/F/A-nn) survives the
sweep. Upstream keeps history via git, instances keep cleanliness via scan.
Refs: US-17; D-005; FAILURES F-002.
## D-005 · 2026-08-25 · Method vs instance: bin/init splits keel into template and workspace
Rationale: keel repo conflated the method with its own dogfood instance —
staff cloning it inherited 14 stories, decision history and failure logs.
A delivery method must instantiate clean per project while staying
upgradeable from upstream.
Consequences: fresh clones run bin/init once -> empty contracts/ledgers,
archive cleared, src/ created as project-code home, method files byte-
identical so pulls deliver improvements without touching instance data.
Refs: US-15; PLAN P3; FAILURES F-002 (local-first mode).

## D-004 · 2026-08-25 · First standing law: laws/stack.md
Rationale: Packs were compiling against an empty laws/ directory, so
execution was not bound to any stack constraint. US-14 seeds the
toolchain rules as a versioned file instead of scattering them in METHOD.
Consequences: every `bin/pack` now inlines `law/stack.md` under Laws;
further stack changes are new versions of this file, not METHOD patches.
Refs: US-14; METHOD §2 `laws/*.md`; bin/keel cmd_pack.

## D-003 · 2026-08-25 · Human overview is a first-class generated projection
Rationale: The operator is always human and needs decision-grade overview
(status, progress, problems) — the Erdt system-plan §06 proved the value;
the CC prototype proved hand-maintained views rot (US-07 manual sync).
Overview must therefore be derived from canonical store, never hand-edited.
Consequences: `bin/render` required in P1; pre-P1 lag is acceptable and
must be labeled; hand-editing overview.html is forbidden.
Refs: METHOD §2; PLAN P1/P2; examples/index.html §06; CC user-stories US-07.

## D-002 · 2026-08-25 · Two audiences, one truth: agent-facing STATE.md, human-facing overview.html
Rationale: Machines need terse bounded text; humans need shaped,
decision-oriented presentation. Neither should contaminate the other.
Both derive from the same store so they cannot disagree structurally.
Consequences: STATE.md optimizes for tokens; overview.html optimizes for
glanceability; both regenerate from data/laws/decisions sources.
Refs: METHOD §0–§2.

## D-001 · 2026-08-25 · Keel adopts file-native canonical store; CC harness-data JSON promoted to schema v0
Rationale: Two experiments triangulated — shopfloor monolith gave
write-path unity but token bloat; separated CC gave read scaling but
forked truth. Canonical JSON + generated projections resolves both.
Consequences: `data/stories.json` seeded with the historical 12-story
corpus (all done) as schema reference; future contracts follow schema v0;
field extensions require an AMENDMENTS entry.
Refs: control-center/agent-harness.html harness-data; examples/index.html;
PLAN.md phases.
