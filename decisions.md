# DECISIONS

Newest first. Format: `## D-nn · YYYY-MM-DD · title` followed by
Rationale / Consequences / Refs. Corrections via supersede, never edits.

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
