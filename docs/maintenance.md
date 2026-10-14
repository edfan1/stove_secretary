# Maintaining the planning graph

## Source of truth

The [index](README.md) maps IDs to definitions; it does not repeat their status. Component responsibilities and gaps live in component documents; semantic interfaces/shared conventions in the contract register; scope and proposed requirements in requirements; unresolved choices and rationale in decisions; evaluation plans/results in experiments. App READMEs, overview, templates, and frozen sources support navigation rather than adding architecture nodes. Future concrete schemas link to semantic contracts. This documentation graph does not prescribe C4 runtime storage.

[Source snapshots](sources/README.md) remain frozen. Working definitions cite evidence and mark proposals, accepted decisions, unresolved choices, and inspected implementation separately. A source statement or implemented choice does not establish team acceptance. Preserve superseded rationale; never reuse IDs. When resolving an Oxx, retain its section and link the decision record. When moving a node, update its index target and inaccurate incoming links. Extend existing sections unless a distinct node is needed; routine implementation details do not require decision records.

## Change-impact workflow

Adapted from [architecture section 6](sources/architecture-v0.1.md#6-change-propagation):

1. Find relevant canonical IDs and source/evaluation evidence through the index.
2. Follow useful links and search incoming references, for example `rg 'IC05|O07' README.md AGENTS.md CONTRIBUTING.md apps contracts hardware evaluation scripts docs --glob '*.md' --glob '!docs/sources/**'`. Frozen source references remain available for provenance review.
3. Inspect contract participants and downstream state, scheduling, presentation, observability, and replay. Consider behavior changes from cadence, evidence quality, prompts, uncertainty, cancellation, and latency even without a payload change. Note unresolved effects; links do not prove complete coverage and cycles are valid.
4. Update the canonical definition once and only references that became inaccurate. Update affected evaluation plans/results and implementation status in the same task; behavior-preserving refactors need no architecture edit unless references change.
5. Check changed links/anchors/index entries and ownership/relationship semantics, then inspect the diff. Preserve IDs and provenance; keep provider details in adapters unless deliberately part of a boundary.

## User-correction navigation example

Source basis: [IC05 correction ownership](sources/architecture-v0.1.md#ic05--session-mutation-and-commit-result), [change propagation](sources/architecture-v0.1.md#6-change-propagation), and [O07](decisions/README.md#o07). This traces the supplied ownership with proposed evaluation links; it does not decide invalidation rules.

A correction such as “I already added the pasta” starts in [C7](components/C7-app-and-interaction-layer.md#c7) through [IC06](contracts/register.md#ic06) to [C5](components/C5-assistant-harness.md#c5). Follow [IC05](contracts/register.md#ic05) to [C4](components/C4-session-state-and-history.md#c4): C4 validates the commit, preserves corrected facts/provenance and affected relationships; C5 reconciles pending model work, timers, and notifications after successful commit.

Review [IC04](contracts/register.md#ic04) revisioned views for [C3](components/C3-observation-selection-and-optional-perception.md#c3), C5, and C7; [IC11](contracts/register.md#ic11) for delayed model proposals; and [IC07](contracts/register.md#ic07) for replacement/cancellation and stale output. [IC08](contracts/register.md#ic08) may need review if the correction changes observation priorities; that effect depends on still-open rules. Follow [C6](components/C6-observability-and-replay.md#c6), [IC09](contracts/register.md#ic09), and [IC10](contracts/register.md#ic10) to trace/replay effects and isolation. Search incoming `IC05` / `O07` references to find component gaps and evaluation entries; follow their requirement and implementation-location links to requirements and app guidance.

[E04](experiments/README.md#e04) proposes wrong-addition, delayed-detection, duplicate, stale-result, timer, and cancellation cases; [E05](experiments/README.md#e05) investigates correction presentation. Neither has results. O07 still leaves derived-belief invalidation and timer reconciliation details unresolved; comprehensive live/user acceptance coverage is missing. Change definitions and affected references only once those rules have evidence or explicit decisions.
