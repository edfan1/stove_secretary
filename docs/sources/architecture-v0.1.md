# Stove Secretary: Architecture and Component Contracts

**Revision:** 0.1 · **Updated:** October 14, 2026 · **Status:** Working design

**Purpose:** Maintain a shared description of the system's components and the interfaces between them. Use this document to assess change impact.

**Basis:** The attached *Stove Secretary Pitch.md* and the subsequent architecture discussion. This document complements the pitch; it does not replace it.

## 1. Scope and decision status

The system observes an ongoing cooking session, maintains an explicit understanding of that session, and provides timely assistance through an app and potentially voice. The initial scope remains one active vessel and bounded cooking workflows. Thermal sensing is optional.

The seven-component decomposition below is the shared working structure. The proposed interface semantics establish a discussion baseline, not implemented APIs or measured guarantees.

| Status | Meaning |
| --- | --- |
| Working baseline | A boundary or responsibility established in the discussion; change deliberately when needed. |
| Proposed contract | Suggested external behavior to review before implementation. |
| Open decision | A choice that remains unresolved and may affect multiple components. |

**Working baseline:** Capture and buffering run locally; interpretation is model-centered with optional local perception; tools, scheduling, and intervention policy belong under the harness; state is explicit and shared; observability spans the system.

**Still open:** Exact deployment, model/provider selection, speech processing, detailed recipe representation, observation cadence, retention policy, and numerical performance targets. Functional components may share a process or device. A boundary here does not require a separate service.

## 2. Component directory

| ID | Component | Core responsibility | Contracts |
| --- | --- | --- | --- |
| C1 | Physical sensing module | Acquire physical signals and report device condition | IC01, IC09 |
| C2 | Capture and observation management | Organize and retain timestamped evidence | IC01, IC02, IC09 |
| C3 | Observation selection and optional perception | Select evidence and optionally propose detected events | IC02, IC03, IC04, IC08, IC09, IC10 |
| C4 | Session state and history | Maintain authoritative recorded state and validate changes | IC04, IC05, IC09 |
| C5 | Assistant harness | Coordinate model reasoning, tools, scheduling, and interventions | IC02–IC08, IC09–IC11 |
| C6 | Observability and replay | Explain behavior and reproduce sessions for development | IC09, IC10 |
| C7 | App and interaction layer | Accept user input and present system outputs | IC04, IC06, IC07, IC09 |

Contract IDs identify logical interfaces. Their meanings are defined once in Section 4; component descriptions refer to those definitions.

### C1 — Physical sensing module

**Purpose:** Obtain usable camera, thermal, and potentially microphone signals from the cooking environment.

**Owns:** Sensors, mounting, enclosure, power, device drivers, basic device configuration, and hardware health reporting. Local computing hardware may host other components without absorbing their logical responsibilities.

**Receives:** Physical signals and capture/device configuration through IC01.

**Provides:** Device samples and health information to C2 through IC01. Device telemetry may reach C6 through the capture software under IC09.

**Boundary:** Semantic cooking interpretation belongs downstream. The camera being operational does not establish that the vessel is visible or that the scene is interpretable.

**Failure behavior:** Report unavailable or failed sensors explicitly. RGB-only operation remains possible when thermal hardware is absent. Preserve the distinction between an optional sensor and one that unexpectedly disconnected.

**Change impact:** Sensor format, calibration, orientation, or timing changes primarily affect C2 and may alter C3's evidence quality. Mounting changes can invalidate prior evaluation results even when software contracts are unchanged.

### C2 — Capture and observation management

**Purpose:** Convert device output into timestamped, retrievable evidence.

**Owns:** Capture sessions, timestamps and synchronization, rolling buffers, evidence identifiers, sensor metadata, and nonsemantic preprocessing such as resizing. Any calibration used to align sensors belongs in this responsibility.

**Receives:** Device data from C1 through IC01; evidence access requests from C3 and C5 through IC02.

**Provides:** Normalized observations, evidence references, and availability status through IC02. Publishes operational telemetry through IC09.

**Boundary:** Structuring frames is distinct from interpreting what happened in them. Capture rate and analysis rate are independently configurable.

**Failure behavior:** Report gaps, unsynchronized samples, expired references, and disconnected devices. Do not substitute an old frame while presenting it as current. Evidence-retention duration is still open.

**Change impact:** Preprocessing, timestamps, or evidence-access changes affect C3, C5, and replay. Changes to buffer retention affect how far backward the assistant can inspect a session.

### C3 — Observation selection and optional perception

**Purpose:** Determine which evidence deserves analysis and optionally extract inexpensive candidate events.

**Owns:** Sampling policies, frame/clip selection, vessel-region crops, local motion/change processing, and any dedicated detectors. Candidate events remain hypotheses until accepted into state.

**Receives:** Observations from C2 through IC02; state views from C4 through IC04; analysis priorities from C5 through IC08. Replay can supply equivalent observations through IC10.

**Provides:** Selected evidence and optional candidate events to C5 through IC03; telemetry through IC09.

**Boundary:** Does not directly modify authoritative session state or issue cooking advice. Local perception is replaceable; the system does not depend on a fixed catalog of specialized detectors.

**Failure behavior:** Include visibility limitations and missing evidence. A missing detection is not proof that an action did not occur. Periodic observation provides a path for discovering changes missed by event triggers.

**Change impact:** Selection or detector changes affect C5's available evidence, state accuracy, prompt timing, and evaluation. A compatible payload can still materially change behavior and cost.

### C4 — Session state and history

**Purpose:** Maintain the authoritative record of the system's knowledge, beliefs, and commitments during a cook.

**Owns:** Session lifecycle, goal/plan representation, vessel identities, event history, current state, evidence provenance, uncertainty, corrections, and recorded timers or other commitments.

**Receives:** State queries through IC04 and proposed mutations from C5 through IC05.

**Provides:** Versioned state/history views through IC04 and mutation results through IC05; telemetry through IC09.

**Boundary:** Authoritative means authoritative for the system's recorded understanding, not guaranteed physical truth. Observations, user statements, inferences, and executed actions remain distinguishable. C5 owns scheduling and execution; C4 owns their recorded status.

**Failure behavior:** Reject invalid updates, stale conflicting changes, and duplicate actions according to the mutation contract. Preserve corrections and their effects in history. Missing information remains unknown.

**Change impact:** State-field meanings or lifecycle changes affect C3, C5, C7, and replay. Corrections may invalidate derived beliefs or actions; the ownership of that reconciliation is described under IC05.

### C5 — Assistant harness

**Purpose:** Coordinate reasoning and assistance across observations, state, user input, and scheduled events.

**Owns:** Trigger handling, context assembly, model adapters, tool exposure/execution, scheduling, intervention policy, and routing outputs. Optional speech recognition and synthesis adapters live here provisionally.

**Receives:** Selected observations through IC03; evidence through IC02; state through IC04; user interactions through IC06; model responses through IC11; replay inputs through IC10. Scheduled triggers originate within the harness.

**Provides:** State mutations through IC05; analysis priorities through IC08; presentation requests through IC07; model requests through IC11; telemetry through IC09.

**Internal responsibilities:**

- Resolve triggers and assemble relevant evidence and state.
- Request model interpretation and validate proposed actions.
- Execute tools with identifiable, duplicate-resistant actions.
- Maintain observation checkpoints and user timers.
- Decide whether an intervention is grounded, timely, and worth an interruption.
- Handle corrections, session stop, and cancellation of obsolete work.

**Boundary:** Models propose interpretations and actions; backend logic applies validated changes and executes them. An established timer can expire without another model call.

**Failure behavior:** Handle model timeouts, invalid responses, stale reasoning, and delivery failures explicitly. Recheck relevant state before applying delayed results. Preserve local/basic timer behavior where supported during model unavailability; exact deployment determines the failure boundary.

**Change impact:** Tools, scheduling, or intervention changes can affect C4, C7, C3, provider adapters, and evaluation. A prompt-only change may alter externally visible behavior and belongs in the change record.

### C6 — Observability and replay

**Purpose:** Make behavior explainable and support repeatable development experiments.

**Owns:** Correlated telemetry, session traces, evaluation exports, replay records, and development replay entry points. Operational telemetry and opt-in retained cooking evidence have separate retention decisions.

**Receives:** Instrumentation from the other components through IC09.

**Provides:** Diagnostic and evaluation views; development replay inputs through IC10.

**Boundary:** Does not act as a second source of live session state. Replay preserves recorded inputs and timing; derived outputs are recomputed for comparison unless an explicitly recorded dependency is being replayed.

**Failure behavior:** Missing telemetry should be visible. A telemetry failure should not unnecessarily block assistance. Replay must isolate timers and notifications from the real live session.

**Change impact:** Trace or replay format changes affect instrumentation across components and comparability with previous experiments.

### C7 — App and interaction layer

**Purpose:** Let users start and guide sessions, inspect beliefs, manage timers, and receive assistance.

**Owns:** App presentation, session controls, typed input, correction UI, notification display, delivery feedback, and relevant mute/privacy controls. A phone-based voice interface may also live here.

**Receives:** State views through IC04 and output requests through IC07.

**Provides:** Questions, goals, corrections, controls, and delivery/user feedback through IC06; telemetry through IC09.

**Boundary:** UI changes to authoritative session state go through C5 and IC05. Direct reads from C4 are a logical dependency and need not be a direct network connection.

**Failure behavior:** Indicate disconnected or stale state. Distinguish a pending action from one confirmed by the backend. Do not treat successful display or audio playback as proof that the cook acted.

**Change impact:** New interaction types affect C5 and sometimes C4. Changes to presentation or acknowledgments affect notification behavior and usefulness measurements.

## 3. Shared conventions

These are proposed semantic conventions, not final JSON schemas.

- **Session identity:** Session-scoped records carry a stable session identifier. Vessel-scoped claims identify the relevant vessel where known.
- **Record identity:** Observations, events, interactions, and actions have stable identifiers so retries and repeated recognition can be distinguished from new activity.
- **Time:** Preserve occurrence/capture time separately from processing time. Cross-device timestamp alignment belongs to C2; precision and synchronization requirements remain open.
- **Evidence:** Interpretations identify their supporting observations or user statements. Evidence references can expire; callers receive explicit availability status.
- **Uncertainty:** Distinguish confirmed information, candidate interpretations, and unknown state. Model-supplied confidence is not assumed to be calibrated.
- **Versions:** State views identify the revision they represent. Delayed reasoning can be checked against subsequent changes.
- **Errors:** Report unavailability, invalid input, expiration, rejection, and timeout distinctly. Silence must not be interpreted as a negative observation or successful action.
- **Lifecycle:** Start, pause, resume, and stop semantics must be coordinated. Stopping a session prevents obsolete work from creating new user-facing effects. Detailed pause behavior remains open.

## 4. External contract register

“External” means across a component boundary. These contracts may be function calls, messages, streams, or network APIs. Transport is intentionally unspecified.

The **owner** maintains the interface definition and coordinates changes with its consumers. This is architectural ownership, not an assigned team member.

| ID | Interface | Direction / participants | Owner |
| --- | --- | --- | --- |
| IC01 | Device acquisition and configuration | C1 ↔ C2 | C1 |
| IC02 | Normalized observation access | C2 ↔ C3, C5 | C2 |
| IC03 | Selected evidence and candidate events | C3 → C5 | C3 |
| IC04 | Session state/history access | C4 ↔ C3, C5, C7 | C4 |
| IC05 | Session mutation and commit result | C5 ↔ C4 | C4 |
| IC06 | User interaction and feedback | C7 → C5, with acceptance result | C5 |
| IC07 | Presentation and notification request | C5 → C7 | C7 |
| IC08 | Observation priorities | C5 → C3, with acceptance result | C3 |
| IC09 | Instrumentation | C1–C5, C7 → C6 | C6 |
| IC10 | Development replay | C6 → C3, C5 | C6 |
| IC11 | Model interaction | C5 ↔ external model provider | C5 adapter |

### IC01 — Device acquisition and configuration

**Carries:** Device identity, modality, raw sample/stream, available timing metadata, format, configuration, and health. Configuration covers capabilities the actual device exposes.

**Guarantee:** Make missing sensors and sample gaps distinguishable from valid samples. Report when requested configuration cannot be applied. Calibration metadata accompanies the relevant sensor configuration.

**Open:** Device protocols, supported rates/resolutions, thermal units/calibration, and microphone/speaker placement. Physical speaker output needs a separate device-output contract if implemented in C1; IC01 currently covers acquisition only.

### IC02 — Normalized observation access

**Carries:** Observation identity, modality, capture time or interval, source, evidence reference/content, preprocessing metadata, and availability/quality status. Supports both fresh observations and lookup of buffered evidence by time or identifier.

**Guarantee:** Returned timestamps describe capture, not request completion. Missing or expired evidence is explicit. Images/audio retain enough provenance to relate them to the session timeline.

**Open:** Stream versus query transport, buffering duration, media representation, and maximum retrieval size. Physical microphone audio may route from C2 directly to C5's speech adapter; it does not require visual-selection logic in C3.

### IC03 — Selected evidence and candidate events

**Carries:** Selection identity, session/vessel context where known, time interval, evidence references, reason for selection, visibility limitations, and optional detector-produced candidates with their source and uncertainty.

**Guarantee:** Separate selected evidence from claims about that evidence. Repeated selections may reference the same event; C5 must not assume each arrival represents a distinct cooking action.

**Open:** Initial sampling cadence, detector catalog, latency targets, and calibrated acceptance criteria.

### IC04 — Session state/history access

**Carries:** A state/history query and a revisioned result containing relevant goals, plans, vessel state, events, commitments, corrections, and provenance. Views may be scoped to avoid returning the entire history.

**Guarantee:** Each returned view represents a coherent identified revision. Unknown and stale information remain distinguishable. Consumers can tell which state informed a selection or model call.

**Open:** Polling versus subscriptions, recipe/plan representation, state-field schemas, and history retention. The app may access a filtered view through the backend.

### IC05 — Session mutation and commit result

**Carries:** A proposed change, originating trigger/action identifier, relevant state revision, supporting evidence, and a result stating accepted/rejected/duplicate with the resulting revision or reason.

**Guarantee:** C4 validates and applies changes consistently. Conflicting stale changes do not silently overwrite newer facts. Repeated execution of the same action does not create another commitment. Corrections preserve history rather than erasing their provenance.

**Correction ownership:** C4 records the corrected fact and its affected relationships. C5 reconciles pending model work, scheduled timers, and notifications with that change. For the first version, C5 can perform explicit reconciliation after a successful correction commit.

**Open:** Exact mutation vocabulary, conflict-resolution rules, and how derived beliefs are marked invalid or recomputed.

### IC06 — User interaction and feedback

**Carries:** Interaction identity, session, time, input type, content or structured control, and relevant referenced event/timer/notification. Types include goal, question, correction, timer command, session control, and delivery/usefulness feedback.

**Guarantee:** C5 reports acceptance or rejection so C7 can distinguish pending from accepted input. Corrections enter shared state. Notification delivery, acknowledgment, and evidence of user action remain distinct.

**Open:** Voice turn handling, interruption behavior, and the set of structured controls. Speech transcribed from physical microphone input joins the same interaction handling inside C5.

### IC07 — Presentation and notification request

**Carries:** Output identity, session, output type/content, relevant state/action references, delivery channel, and any expiration, cancellation, or replacement relationship needed for timely output.

**Guarantee:** Avoid duplicate presentation for the same output identifier. Obsolete or stopped-session messages must not be presented as current. Delivery feedback returns through IC06.

**Open:** Speech generation location, message priority rules, disconnected delivery, and channel fallback. App speech and device speech may require separate adapters while preserving shared output semantics.

### IC08 — Observation priorities

**Carries:** Requested target/vessel, observation objective, relevant state revision, and a bounded requested cadence or follow-up time. Example: inspect the pot more frequently while waiting for a boil.

**Guarantee:** C3 can accept, reject, or constrain a request based on capabilities and resources. Expired or canceled priorities do not remain active indefinitely. A priority cannot guarantee successful observation of the requested event.

**Open:** Scheduling ownership at the detailed implementation level: C5 owns semantic checkpoints; C3 may manage sampling cadence within those priorities. Requested bounds and resource limits remain undecided.

### IC09 — Instrumentation

**Carries:** Component identity, session/trace correlation, timestamps, observation/event/action references, operation outcome, error information, and relevant latency or model usage measurements. Content retention is configurable separately from metadata logging.

**Guarantee:** Traces distinguish proposed actions, committed changes, execution, and delivery. They expose the state revision and evidence used for model decisions without requiring hidden model reasoning.

**Open:** Storage, access, retention, redaction, and capture of prompts/media. Replay requiring retained evidence must respect the recording configuration.

### IC10 — Development replay

**Carries:** Recorded normalized observations and interactions, their original timing, initial state/setup, and explicit choices about recorded versus live external dependencies. Reuses IC02/IC06 semantics wherever possible.

**Guarantee:** Replay operates in an isolated session and uses an explicit time basis for timers and observation checkpoints. It cannot deliver real cooking alerts or mutate the active live session. New system outputs remain distinguishable from recorded outputs.

**Open:** Playback speed, replay clock, dependency substitution, and evaluation export format. Model nondeterminism and dependency versions should be recorded when comparing runs.

### IC11 — Model interaction

**Carries:** Relevant instructions, state revision/context, selected evidence, user request/trigger, and available tool definitions; receives proposed interpretations, answers, tool calls, or explicit inability to answer.

**Guarantee:** C5 validates responses and tool arguments before effects occur, handles timeout/invalid output, and rechecks relevant state for delayed results. Provider output never bypasses IC05 or action validation.

**Open:** Provider, model, request format, response structure, budget, and latency targets. Provider-specific schemas belong in adapters rather than the seven-component architecture. Separate speech-provider contracts can be added once that choice is made.

## 5. Ownership rules that span components

| Concern | Primary owner | Other responsibilities |
| --- | --- | --- |
| What was physically captured and when | C2 | C1 supplies device samples and timing capabilities |
| Which evidence receives analysis | C3 | C5 supplies priorities; C4 supplies context |
| What the system currently records as known/believed | C4 | C5 proposes validated updates and reconciles consequences |
| What action should occur and when | C5 | C4 records commitments; C7 presents effects |
| What the user said or corrected | C5 interaction handling | C7 captures app input; C4 persists relevant facts/history |
| Whether an output was delivered | C7 or output adapter | C5 records feedback; delivery does not imply action |
| Why the system behaved as it did | C6 trace infrastructure | Every component supplies correlated instrumentation |

These rules prevent multiple components from independently owning the same fact or timer. Timer definitions/status live in C4; active scheduling and expiry execution live in C5.

## 6. Change propagation

For each substantive change:

1. Identify affected component and contract IDs.
2. Update the owned contract definition once, then review every participant listed in the register.
3. Record semantic effects even if the payload format is unchanged: cadence, evidence quality, uncertainty, latency, and cancellation behavior can affect consumers.
4. Check downstream consequences using the component descriptions, including state, user output, and replay.
5. Update relevant evaluation cases and, once it exists, the diagram using the same IDs.
6. Record the decision, unresolved consequences, and current revision here. Keep low-level API schemas in implementation documentation and link them when available.

| Example change | Immediate boundary | Propagation to review |
| --- | --- | --- |
| Add a thermal sensor | C1/C2, IC01/IC02 | Alignment, C3 selection, C5 evidence interpretation, replay, sensor-value evaluation |
| Increase observation cadence | C3, IC03/IC08 | C5 workload/cost, duplicate events, latency, intervention frequency |
| Replace local detectors with model interpretation | C3/C5, IC03/IC11 | State-update evidence, model load, uncertainty, comparable replay cases |
| Add a second vessel | C3/C4, IC03–IC05 | Identity tracking, tool targeting, timers, app presentation, multi-vessel evaluation |
| Change timer-start semantics | C5/C4, IC05–IC07 | Correction recovery, duplicate prevention, displayed timing, replay clock |
| Change buffer retention | C2, IC02 | Retrospective questions, evidence expiration, trace references, replay availability |
| Add hood-mounted speaker output | C1/C5 | New physical output contract, speech adapter, delivery feedback, mute behavior |

### Change-record template

- **Change ID / date:**
- **Problem and intended behavior:**
- **Affected components:**
- **Affected contracts:**
- **Compatibility and behavioral impact:**
- **Consumers reviewed / required updates:**
- **Evaluation evidence or planned check:**
- **Remaining questions:**
- **Decision status / document revision:**

## 7. Open decisions

| ID | Decision | Affected areas | Next useful evidence |
| --- | --- | --- | --- |
| O01 | Where C4/C5 execute and what survives connectivity loss | C2, C4, C5, C7 | Target hardware and offline requirements |
| O02 | Initial recipe/goal representation | C4, C5, C7 | A few complete target workflows and clarification examples |
| O03 | Sampling strategy and required temporal evidence | C2, C3, C5 | Labeled recordings with missed/occluded events |
| O04 | Speech hardware and recognition/synthesis location | C1, C2, C5, C7 | Voice prototype under cooking/hood noise |
| O05 | Whether thermal adds useful information | C1–C3, C5 | RGB-only versus thermal-assisted trials on target cookware |
| O06 | Retention, consent, and replay dataset policy | C2, C6, C7 | Recording controls and evaluation needs |
| O07 | Correction invalidation and timer reconciliation rules | C4, C5, C7 | Wrong-addition, delayed-detection, and duplicate-trigger cases |
| O08 | Performance and intervention targets | C3, C5, C6 | Pilot measurements of delay, state accuracy, and prompt usefulness |

## 8. Revision history

| Revision | Date | Description |
| --- | --- | --- |
| 0.1 | October 14, 2026 | Initial seven-component working architecture, proposed external contracts, ownership boundaries, and change-impact register. |

## 9. Future diagram boundary

When drawing the diagram, reuse C1–C7 as component labels and IC01–IC11 as interface labels. Show observations, user interactions, and scheduled triggers as distinct inputs to C5; show feedback from state and harness into observation selection. Treat C6 as cross-cutting and external model services as dependencies of C5. Add deployment locations only after resolving O01/O04.

The diagram should summarize this document. Contract details and open decisions remain here so changes can be maintained without crowding the drawing.
