# Agent guidance

- Start with the [project overview](docs/overview.md) and [documentation index](docs/README.md), then read relevant components, contracts, and decisions. Every task need not read every document.
- Distinguish working design, proposals, unresolved choices, and inspected implementation. Preserve uneven planning maturity; label new discovery questions rather than filling gaps with apparent decisions.
- Maintain docs in the same task when changing responsibilities, contract behavior, scope, substantive decisions, evaluation findings, or implementation status. Behavior-preserving refactors need architecture edits only if references become inaccurate.
- Follow the [maintenance workflow](docs/maintenance.md): update canonical definitions once, follow useful links, and search stable IDs for incoming references. Review affected participants and downstream state, scheduling, presentation, observability, and replay. Sampling and prompt changes can change behavior without changing payloads.
- Extend existing sections when possible. Add a node only for a distinct requirement, choice, decision, or experiment; avoid duplicate definitions, speculative relationships, and records for routine implementation details.
- Preserve IDs, anchors, provenance, and superseded rationale. Update the index and implementation links when moving nodes. Record material code/design disagreements without treating implemented choices as accepted decisions.
- Keep provider/SDK details out of semantic contracts unless deliberately part of the boundary. Run checks appropriate to the change. Do not commit credentials, recordings, or sensitive traces; follow [evaluation data boundaries](evaluation/README.md).
- Treat [source snapshots](docs/sources/README.md) as frozen provenance. Maintain current definitions in canonical planning documents; retain differing source revisions under distinct descriptive filenames.
