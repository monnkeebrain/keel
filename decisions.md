# DECISIONS

Newest first. Format: `## D-nn · YYYY-MM-DD · title` followed by
Rationale / Consequences / Refs. Corrections via supersede, never edits.

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
