# Semantic contract register

**Status:** All IC01–IC11 definitions are proposed contracts from [architecture sections 3–4](../sources/architecture-v0.1.md#4-external-contract-register). **Implementation:** None. “Proposed required behavior” below preserves source guarantee semantics; it is not a measured guarantee. Provider SDKs, schemas, transport, and deployment remain unspecified.

## Shared conventions

These are proposed semantic conventions, not final JSON schemas.

- **Session identity:** Session-scoped records carry a stable session identifier. Vessel-scoped claims identify the relevant vessel where known.
- **Record identity:** Observations, events, interactions, and actions have stable identifiers so retries and repeated recognition can be distinguished from new activity.
- **Time:** Preserve occurrence/capture time separately from processing time. Cross-device timestamp alignment belongs to [C2](../components/C2-capture-and-observation-management.md#c2); precision and synchronization requirements remain open.
- **Evidence:** Interpretations identify their supporting observations or user statements. Evidence references can expire; callers receive explicit availability status.
- **Uncertainty:** Distinguish confirmed information, candidate interpretations, and unknown state. Model-supplied confidence is not assumed to be calibrated.
- **Versions:** State views identify the revision they represent. Delayed reasoning can be checked against subsequent changes.
- **Errors:** Report unavailability, invalid input, expiration, rejection, and timeout distinctly. Silence must not be interpreted as a negative observation or successful action.
- **Lifecycle:** Start, pause, resume, and stop semantics must be coordinated. Stopping a session prevents obsolete work from creating new user-facing effects. Detailed pause behavior remains open.

## Directory

“External” means across a component boundary. These contracts may be function calls, messages, streams, or network APIs. Transport is intentionally unspecified.

The **owner** maintains the interface definition and coordinates changes with its consumers. This is architectural ownership, not an assigned team member.

| ID | Interface | Direction / participants | Owner |
| --- | --- | --- | --- |
| [IC01](#ic01) | Device acquisition and configuration | [C1](../components/C1-physical-sensing-module.md#c1) ↔ [C2](../components/C2-capture-and-observation-management.md#c2) | [C1](../components/C1-physical-sensing-module.md#c1) |
| [IC02](#ic02) | Normalized observation access | [C2](../components/C2-capture-and-observation-management.md#c2) ↔ [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5) | [C2](../components/C2-capture-and-observation-management.md#c2) |
| [IC03](#ic03) | Selected evidence and candidate events | [C3](../components/C3-observation-selection-and-optional-perception.md#c3) → [C5](../components/C5-assistant-harness.md#c5) | [C3](../components/C3-observation-selection-and-optional-perception.md#c3) |
| [IC04](#ic04) | Session state/history access | [C4](../components/C4-session-state-and-history.md#c4) ↔ [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7) | [C4](../components/C4-session-state-and-history.md#c4) |
| [IC05](#ic05) | Session mutation and commit result | [C5](../components/C5-assistant-harness.md#c5) ↔ [C4](../components/C4-session-state-and-history.md#c4) | [C4](../components/C4-session-state-and-history.md#c4) |
| [IC06](#ic06) | User interaction and feedback | [C7](../components/C7-app-and-interaction-layer.md#c7) → [C5](../components/C5-assistant-harness.md#c5), with acceptance result | [C5](../components/C5-assistant-harness.md#c5) |
| [IC07](#ic07) | Presentation and notification request | [C5](../components/C5-assistant-harness.md#c5) → [C7](../components/C7-app-and-interaction-layer.md#c7) | [C7](../components/C7-app-and-interaction-layer.md#c7) |
| [IC08](#ic08) | Observation priorities | [C5](../components/C5-assistant-harness.md#c5) → [C3](../components/C3-observation-selection-and-optional-perception.md#c3), with acceptance result | [C3](../components/C3-observation-selection-and-optional-perception.md#c3) |
| [IC09](#ic09) | Instrumentation | [C1](../components/C1-physical-sensing-module.md#c1)–[C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7) → [C6](../components/C6-observability-and-replay.md#c6) | [C6](../components/C6-observability-and-replay.md#c6) |
| [IC10](#ic10) | Development replay | [C6](../components/C6-observability-and-replay.md#c6) → [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5) | [C6](../components/C6-observability-and-replay.md#c6) |
| [IC11](#ic11) | Model interaction | [C5](../components/C5-assistant-harness.md#c5) ↔ external model provider | [C5](../components/C5-assistant-harness.md#c5) adapter |

## IC01

**Interface:** Device acquisition and configuration

**Status:** Proposed; unimplemented. **Owner:** [C1](../components/C1-physical-sensing-module.md#c1) (maintains the definition; not a named team member or deployment host).

**Carries:** Device identity, modality, raw sample/stream, available timing metadata, format, configuration, and health. Configuration covers capabilities the actual device exposes.

**Proposed required behavior:** Make missing sensors and sample gaps distinguishable from valid samples. Report when requested configuration cannot be applied. Calibration metadata accompanies the relevant sensor configuration.

**Open:** Device protocols, supported rates/resolutions, thermal units/calibration, and microphone/speaker placement. Physical speaker output needs a separate device-output contract if implemented in [C1](../components/C1-physical-sensing-module.md#c1); [IC01](#ic01) currently covers acquisition only.

## IC02

**Interface:** Normalized observation access

**Status:** Proposed; unimplemented. **Owner:** [C2](../components/C2-capture-and-observation-management.md#c2) (maintains the definition; not a named team member or deployment host).

**Carries:** Observation identity, modality, capture time or interval, source, evidence reference/content, preprocessing metadata, and availability/quality status. Supports both fresh observations and lookup of buffered evidence by time or identifier.

**Proposed required behavior:** Returned timestamps describe capture, not request completion. Missing or expired evidence is explicit. Images/audio retain enough provenance to relate them to the session timeline.

**Open:** Stream versus query transport, buffering duration, media representation, and maximum retrieval size. Physical microphone audio may route from [C2](../components/C2-capture-and-observation-management.md#c2) directly to [C5](../components/C5-assistant-harness.md#c5)'s speech adapter; it does not require visual-selection logic in [C3](../components/C3-observation-selection-and-optional-perception.md#c3).

## IC03

**Interface:** Selected evidence and candidate events

**Status:** Proposed; unimplemented. **Owner:** [C3](../components/C3-observation-selection-and-optional-perception.md#c3) (maintains the definition; not a named team member or deployment host).

**Carries:** Selection identity, session/vessel context where known, time interval, evidence references, reason for selection, visibility limitations, and optional detector-produced candidates with their source and uncertainty.

**Proposed required behavior:** Separate selected evidence from claims about that evidence. Repeated selections may reference the same event; [C5](../components/C5-assistant-harness.md#c5) must not assume each arrival represents a distinct cooking action.

**Open:** Initial sampling cadence, detector catalog, latency targets, and calibrated acceptance criteria.

## IC04

**Interface:** Session state/history access

**Status:** Proposed; unimplemented. **Owner:** [C4](../components/C4-session-state-and-history.md#c4) (maintains the definition; not a named team member or deployment host).

**Carries:** A state/history query and a revisioned result containing relevant goals, plans, vessel state, events, commitments, corrections, and provenance. Views may be scoped to avoid returning the entire history.

**Proposed required behavior:** Each returned view represents a coherent identified revision. Unknown and stale information remain distinguishable. Consumers can tell which state informed a selection or model call.

**Open:** Polling versus subscriptions, recipe/plan representation, state-field schemas, and history retention. The app may access a filtered view through the backend.

## IC05

**Interface:** Session mutation and commit result

**Status:** Proposed; unimplemented. **Owner:** [C4](../components/C4-session-state-and-history.md#c4) (maintains the definition; not a named team member or deployment host).

**Carries:** A proposed change, originating trigger/action identifier, relevant state revision, supporting evidence, and a result stating accepted/rejected/duplicate with the resulting revision or reason.

**Proposed required behavior:** [C4](../components/C4-session-state-and-history.md#c4) validates and applies changes consistently. Conflicting stale changes do not silently overwrite newer facts. Repeated execution of the same action does not create another commitment. Corrections preserve history rather than erasing their provenance.

**Correction ownership:** [C4](../components/C4-session-state-and-history.md#c4) records the corrected fact and its affected relationships. [C5](../components/C5-assistant-harness.md#c5) reconciles pending model work, scheduled timers, and notifications with that change. For the first version, [C5](../components/C5-assistant-harness.md#c5) can perform explicit reconciliation after a successful correction commit.

**Open:** Exact mutation vocabulary, conflict-resolution rules, and how derived beliefs are marked invalid or recomputed.

## IC06

**Interface:** User interaction and feedback

**Status:** Proposed; unimplemented. **Owner:** [C5](../components/C5-assistant-harness.md#c5) (maintains the definition; not a named team member or deployment host).

**Carries:** Interaction identity, session, time, input type, content or structured control, and relevant referenced event/timer/notification. Types include goal, question, correction, timer command, session control, and delivery/usefulness feedback.

**Proposed required behavior:** [C5](../components/C5-assistant-harness.md#c5) reports acceptance or rejection so [C7](../components/C7-app-and-interaction-layer.md#c7) can distinguish pending from accepted input. Corrections enter shared state. Notification delivery, acknowledgment, and evidence of user action remain distinct.

**Open:** Voice turn handling, interruption behavior, and the set of structured controls. Speech transcribed from physical microphone input joins the same interaction handling inside [C5](../components/C5-assistant-harness.md#c5).

## IC07

**Interface:** Presentation and notification request

**Status:** Proposed; unimplemented. **Owner:** [C7](../components/C7-app-and-interaction-layer.md#c7) (maintains the definition; not a named team member or deployment host).

**Carries:** Output identity, session, output type/content, relevant state/action references, delivery channel, and any expiration, cancellation, or replacement relationship needed for timely output.

**Proposed required behavior:** Avoid duplicate presentation for the same output identifier. Obsolete or stopped-session messages must not be presented as current. Delivery feedback returns through [IC06](#ic06).

**Open:** Speech generation location, message priority rules, disconnected delivery, and channel fallback. App speech and device speech may require separate adapters while preserving shared output semantics.

## IC08

**Interface:** Observation priorities

**Status:** Proposed; unimplemented. **Owner:** [C3](../components/C3-observation-selection-and-optional-perception.md#c3) (maintains the definition; not a named team member or deployment host).

**Carries:** Requested target/vessel, observation objective, relevant state revision, and a bounded requested cadence or follow-up time. Example: inspect the pot more frequently while waiting for a boil.

**Proposed required behavior:** [C3](../components/C3-observation-selection-and-optional-perception.md#c3) can accept, reject, or constrain a request based on capabilities and resources. Expired or canceled priorities do not remain active indefinitely. A priority cannot guarantee successful observation of the requested event.

**Open:** Scheduling ownership at the detailed implementation level: [C5](../components/C5-assistant-harness.md#c5) owns semantic checkpoints; [C3](../components/C3-observation-selection-and-optional-perception.md#c3) may manage sampling cadence within those priorities. Requested bounds and resource limits remain undecided.

## IC09

**Interface:** Instrumentation

**Status:** Proposed; unimplemented. **Owner:** [C6](../components/C6-observability-and-replay.md#c6) (maintains the definition; not a named team member or deployment host).

**Carries:** Component identity, session/trace correlation, timestamps, observation/event/action references, operation outcome, error information, and relevant latency or model usage measurements. Content retention is configurable separately from metadata logging.

**Proposed required behavior:** Traces distinguish proposed actions, committed changes, execution, and delivery. They expose the state revision and evidence used for model decisions without requiring hidden model reasoning.

**Open:** Storage, access, retention, redaction, and capture of prompts/media. Replay requiring retained evidence must respect the recording configuration.

## IC10

**Interface:** Development replay

**Status:** Proposed; unimplemented. **Owner:** [C6](../components/C6-observability-and-replay.md#c6) (maintains the definition; not a named team member or deployment host).

**Carries:** Recorded normalized observations and interactions, their original timing, initial state/setup, and explicit choices about recorded versus live external dependencies. Reuses [IC02](#ic02)/[IC06](#ic06) semantics wherever possible.

**Proposed required behavior:** Replay operates in an isolated session and uses an explicit time basis for timers and observation checkpoints. It cannot deliver real cooking alerts or mutate the active live session. New system outputs remain distinguishable from recorded outputs.

**Open:** Playback speed, replay clock, dependency substitution, and evaluation export format. Model nondeterminism and dependency versions should be recorded when comparing runs.

## IC11

**Interface:** Model interaction

**Status:** Proposed; unimplemented. **Owner:** [C5](../components/C5-assistant-harness.md#c5) adapter (maintains the definition; not a named team member or deployment host).

**Carries:** Relevant instructions, state revision/context, selected evidence, user request/trigger, and available tool definitions; receives proposed interpretations, answers, tool calls, or explicit inability to answer.

**Proposed required behavior:** [C5](../components/C5-assistant-harness.md#c5) validates responses and tool arguments before effects occur, handles timeout/invalid output, and rechecks relevant state for delayed results. Provider output never bypasses [IC05](#ic05) or action validation.

**Open:** Provider, model, request format, response structure, budget, and latency targets. Provider-specific schemas belong in adapters rather than the seven-component architecture. Separate speech-provider contracts can be added once that choice is made.
