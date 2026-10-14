# Contributing

Start with the [overview](docs/overview.md) and relevant nodes in the [index](docs/README.md). Inspect Git status and existing instructions before editing; preserve unrelated work. Use a focused branch and reviewable diff, describe the intended behavior and checks, and keep implementation and affected docs in the same change. There are no application setup, test, or build commands yet.

For responsibility or interface changes, follow the [maintenance workflow](docs/maintenance.md), review listed participants, and describe compatibility and downstream behavior even if payloads stay unchanged. Mark a proposal or unresolved consequence explicitly; use a [decision record](docs/templates/decision.md) for a substantive choice and an [experiment record](docs/templates/experiment.md) for distinct evaluation work. Implemented choices alone do not establish team acceptance. Retain open-decision IDs when linking resolutions.

Preserve frozen [sources](docs/sources/README.md), stable IDs, and existing rationale. Keep definitions in their canonical homes and link to them. For documentation changes, check relative links, anchors, index entries, ownership/relationships, and provenance. For code changes, run appropriate checks once tooling exists and state any skipped checks.

Before review, inspect the diff and untracked files for secrets, recordings, and build outputs. Keep environment examples free of credentials. [Evaluation](evaluation/README.md) documents local data locations and recording boundaries. Commit/push actions follow the task's authorization; this initial bootstrap does neither.
