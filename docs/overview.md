# Project overview

Stove Secretary asks whether non-contact sensing and remembered context can provide useful guidance during an ordinary cook without a specially authored sensor-aware recipe. A module near the hood obtains camera evidence, optional thermal evidence, and potentially microphone input; an explicit session record and multimodal assistant support app interaction and potentially voice. Motivation and proposed evaluation come from the [pitch](sources/pitch.md#the-pitch).

## Scope and current status

The initial scope is one cooktop, a fixed overhead camera, one active vessel, and bounded repeatable workflows. The pitch proposes two dishes and a sauce/pasta demonstration with an unstructured session; examples are not selected dishes. This assistance prototype does not replace ventilation, control gas/heat, alter mains wiring, or act as a certified fire/food-safety monitor. See [requirements](requirements.md) for source-backed scope and proposed behavior, including privacy/offline proposals.

Applications are unimplemented: the inspected initial repository held only a minimal README. This bootstrap adds planning and placeholders. Architecture revision 0.1 remains working design; proposed contracts are required behavior to review and test, not proven guarantees.

Planning coverage is uneven. Backend/state/harness concerns received substantially more attention than physical sensing, device integration, and app/user experience. This reflects planning effort, not relative complexity, importance, or readiness. Even detailed backend design is provisional. C1–C7 and IC01–IC11 provide an initial organizing structure; later sensing and interaction work may expose additional responsibilities or contracts. Canonical [C1 gaps](components/C1-physical-sensing-module.md#design-gaps), [C2 gaps](components/C2-capture-and-observation-management.md#design-gaps), and [C7 gaps](components/C7-app-and-interaction-layer.md#design-gaps) remain visible.

## Working architecture

Interpretation is LLM-first: local capture/buffering produces evidence; optional local perception and selection feed a harness that assembles context, calls models, validates tools, and routes assistance. State is explicit and shared; observability spans the system. See the [architecture baseline](sources/architecture-v0.1.md#1-scope-and-decision-status).

| Component | Summary |
| --- | --- |
| [C1](components/C1-physical-sensing-module.md#c1) | Physical signals and device condition |
| [C2](components/C2-capture-and-observation-management.md#c2) | Timestamped evidence and buffers |
| [C3](components/C3-observation-selection-and-optional-perception.md#c3) | Selected evidence and optional candidate events |
| [C4](components/C4-session-state-and-history.md#c4) | Authoritative recorded understanding and history |
| [C5](components/C5-assistant-harness.md#c5) | Reasoning, validated tools, scheduling, and interventions |
| [C6](components/C6-observability-and-replay.md#c6) | Traces and isolated development replay |
| [C7](components/C7-app-and-interaction-layer.md#c7) | User interaction and presentation |

The [contract register](contracts/register.md) owns interface semantics and shared conventions. Logical boundaries may be calls, streams, messages, or APIs and need not correspond to services. Proposed allocation is local acquisition/buffering in [device](../apps/device/README.md), state/harness in [backend](../apps/backend/README.md), presentation in [mobile](../apps/mobile/README.md). C3 may span device/backend; C6 spans instrumentation and [evaluation](../evaluation/README.md). Deployment and disconnected behavior remain [O01](decisions/README.md#o01); speech placement remains [O04](decisions/README.md#o04).

## Source reconciliation

- The pitch's fast-perception stage and detector-oriented development gates are historical proposals. The newer architecture makes local perception optional and uses a model-centered harness; do not interpret the pitch as a mandatory detector catalog. Detection and grounding metrics can evaluate model-centered behavior too.
- The pitch describes one or two vessels in the experience, but its bounded first version and the architecture specify one active vessel. Multi-vessel work remains an extension.
- The pitch proposes offline logging/basic timers and local video processing where practical. The architecture leaves deployment and what survives loss of connectivity open. Preserve those proposals without claiming an established offline implementation.
- The pitch suggests web or mobile and later voice. The mobile folder is provisional, not a native-app decision; speech remains open. Proposed team ownership and quantitative targets have not become accepted decisions merely by being in a source.

One supplied architecture revision was found; it agrees with the bootstrap's architectural summaries. [Sources](sources/README.md) retain historical content unchanged. D001 records the user's accepted monorepo choice; other open choices are in the [decision register](decisions/README.md).

## Navigation and maintenance

Start here, then use the [ID index](README.md) to read relevant components, contracts, decisions, and experiments. The graph is the documents and meaningful links, not a runtime state graph or separate database. Relevant changes update canonical definitions and affected references in the same task using the [maintenance workflow](maintenance.md). Incoming ID searches help discover effects; links are not proof of complete impact coverage.

Next proposed implementation is a replay-first slice through evidence, state, harness output, and a simple client ([E04](experiments/README.md#e04)). Placement/evidence trials ([E01](experiments/README.md#e01), [E02](experiments/README.md#e02)) and interaction prototyping ([E05](experiments/README.md#e05)) remain necessary alongside it.
