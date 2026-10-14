# App and interaction layer

## C7

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c7--app-and-interaction-layer); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail covers logical interaction boundaries and delivery distinctions. Cooking workflows, setup, UI design, client technology, and delivery mechanics remain unspecified.

### Supplied responsibilities and behavior

**Purpose:** Let users start and guide sessions, inspect beliefs, manage timers, and receive assistance.

**Owns:** App presentation, session controls, typed input, correction UI, notification display, delivery feedback, and relevant mute/privacy controls. A phone-based voice interface may also live here.

**Receives:** State views through [IC04](../contracts/register.md#ic04) and output requests through [IC07](../contracts/register.md#ic07).

**Provides:** Questions, goals, corrections, controls, and delivery/user feedback through [IC06](../contracts/register.md#ic06); telemetry through [IC09](../contracts/register.md#ic09).

**Boundary:** UI changes to authoritative session state go through [C5](C5-assistant-harness.md#c5) and [IC05](../contracts/register.md#ic05). Direct reads from [C4](C4-session-state-and-history.md#c4) are a logical dependency and need not be a direct network connection.

**Failure behavior:** Indicate disconnected or stale state. Distinguish a pending action from one confirmed by the backend. Do not treat successful display or audio playback as proof that the cook acted.

**Change impact:** New interaction types affect [C5](C5-assistant-harness.md#c5) and sometimes [C4](C4-session-state-and-history.md#c4). Changes to presentation or acknowledgments affect notification behavior and usefulness measurements.

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| queries | [IC04](../contracts/register.md#ic04) | Filtered state through backend if appropriate. |
| produces / receives acceptance | [IC06](../contracts/register.md#ic06) | User input and delivery/usefulness feedback to [C5](C5-assistant-harness.md#c5). |
| consumes | [IC07](../contracts/register.md#ic07) | Presentation requests from [C5](C5-assistant-harness.md#c5); [C7](#c7) owns this interface. |
| produces | [IC09](../contracts/register.md#ic09) | Interaction/delivery telemetry. |
| evaluated by | [E05](../experiments/README.md#e05) | Setup, correction, consent, and interaction usability. |

### Implementation location and open decisions

**Provisional location:** [mobile](../../apps/mobile/README.md); web-versus-native choice remains open. Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O01](../decisions/README.md#o01), [O02](../decisions/README.md#o02), [O04](../decisions/README.md#o04), [O06](../decisions/README.md#o06), [O07](../decisions/README.md#o07). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | Which goal, clarification, and correction workflows are needed? [O02](../decisions/README.md#o02) / [O07](../decisions/README.md#o07). | [C4](C4-session-state-and-history.md#c4), [C5](C5-assistant-harness.md#c5), [C7](#c7); [IC04](../contracts/register.md#ic04)–[IC07](../contracts/register.md#ic07) | Walk complete target workflows and correction examples with a simple interaction prototype ([E05](../experiments/README.md#e05) / [E04](../experiments/README.md#e04)). |
| Source-backed | What disconnected, speech, and recording-control behavior is useful? [O01](../decisions/README.md#o01) / [O04](../decisions/README.md#o04) / [O06](../decisions/README.md#o06). | [C1](C1-physical-sensing-module.md#c1), [C2](C2-capture-and-observation-management.md#c2), [C5](C5-assistant-harness.md#c5), [C6](C6-observability-and-replay.md#c6), [C7](#c7); [IC02](../contracts/register.md#ic02), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07), [IC09](../contracts/register.md#ic09) | Offline scenario review, noisy voice trial, and consent controls ([E07](../experiments/README.md#e07) / [E06](../experiments/README.md#e06) / [E05](../experiments/README.md#e05)). |
| New proposed discovery | How should pairing/setup and session start/pause/resume/stop expose pending versus confirmed state? | [C2](C2-capture-and-observation-management.md#c2), [C4](C4-session-state-and-history.md#c4), [C5](C5-assistant-harness.md#c5), [C7](#c7); [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06) | Prototype setup and controls with cooks; discuss unresolved pause semantics ([E05](../experiments/README.md#e05)). |
| New proposed discovery | How should stale beliefs, correction history, and timers be presented during a cook? | [C4](C4-session-state-and-history.md#c4), [C5](C5-assistant-harness.md#c5), [C7](#c7); [IC04](../contracts/register.md#ic04)–[IC07](../contracts/register.md#ic07) | Compare state/timeline sketches and touch/voice interaction in target workflows ([E05](../experiments/README.md#e05)). |
| New proposed discovery | What background-delivery, expiration, and mute behavior fits the cooking workflow? | [C5](C5-assistant-harness.md#c5), [C7](#c7); [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07) | Prototype foreground/background and disconnect transitions; discuss delivery feedback versus user action ([E05](../experiments/README.md#e05) / [E07](../experiments/README.md#e07)). |
