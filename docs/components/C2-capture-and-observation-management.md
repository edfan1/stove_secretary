# Capture and observation management

## C2

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c2--capture-and-observation-management); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail covers evidence ownership and failure semantics. Capture implementation, synchronization precision, calibration procedure, and connectivity behavior are not specified.

### Supplied responsibilities and behavior

**Purpose:** Convert device output into timestamped, retrievable evidence.

**Owns:** Capture sessions, timestamps and synchronization, rolling buffers, evidence identifiers, sensor metadata, and nonsemantic preprocessing such as resizing. Any calibration used to align sensors belongs in this responsibility.

**Receives:** Device data from [C1](C1-physical-sensing-module.md#c1) through [IC01](../contracts/register.md#ic01); evidence access requests from [C3](C3-observation-selection-and-optional-perception.md#c3) and [C5](C5-assistant-harness.md#c5) through [IC02](../contracts/register.md#ic02).

**Provides:** Normalized observations, evidence references, and availability status through [IC02](../contracts/register.md#ic02). Publishes operational telemetry through [IC09](../contracts/register.md#ic09).

**Boundary:** Structuring frames is distinct from interpreting what happened in them. Capture rate and analysis rate are independently configurable.

**Failure behavior:** Report gaps, unsynchronized samples, expired references, and disconnected devices. Do not substitute an old frame while presenting it as current. Evidence-retention duration is still open.

**Change impact:** Preprocessing, timestamps, or evidence-access changes affect [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](C5-assistant-harness.md#c5), and replay. Changes to buffer retention affect how far backward the assistant can inspect a session.

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| consumes / configures | [IC01](../contracts/register.md#ic01) | Acquisition boundary with [C1](C1-physical-sensing-module.md#c1). |
| produces / serves queries | [IC02](../contracts/register.md#ic02) | Normalized evidence for [C3](C3-observation-selection-and-optional-perception.md#c3)/[C5](C5-assistant-harness.md#c5), including physical audio for [C5](C5-assistant-harness.md#c5). |
| produces | [IC09](../contracts/register.md#ic09) | Capture telemetry to [C6](C6-observability-and-replay.md#c6). |
| evaluated by | [E02](../experiments/README.md#e02) | Temporal evidence and retrievability. |

### Implementation location and open decisions

**Provisional location:** [device](../../apps/device/README.md) for local capture/buffering; backend evidence-access integration remains open. Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O01](../decisions/README.md#o01), [O03](../decisions/README.md#o03), [O04](../decisions/README.md#o04), [O05](../decisions/README.md#o05), [O06](../decisions/README.md#o06). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | What timing, buffering, and evidence-retention choices support temporal reasoning? [O03](../decisions/README.md#o03) / [O06](../decisions/README.md#o06). | [C2](#c2), [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](C5-assistant-harness.md#c5), [C6](C6-observability-and-replay.md#c6); [IC02](../contracts/register.md#ic02), [IC10](../contracts/register.md#ic10) | Labeled missed/occluded events and recording controls ([E02](../experiments/README.md#e02) / [E05](../experiments/README.md#e05)). |
| Source-backed | What local logging survives disconnect? [O01](../decisions/README.md#o01). How does physical audio enter speech handling? [O04](../decisions/README.md#o04). | [C2](#c2), [C5](C5-assistant-harness.md#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02) | Target hardware/offline scenario review and noisy voice prototype ([E07](../experiments/README.md#e07) / [E06](../experiments/README.md#e06)). |
| New proposed discovery | How will calibration validity and cross-sensor alignment be checked after moving or cleaning the mount? | [C1](C1-physical-sensing-module.md#c1), [C2](#c2), [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](C5-assistant-harness.md#c5); [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02) | Repeated placement/calibration trials; inspect evidence quality metadata ([E01](../experiments/README.md#e01)). |
| New proposed discovery | How should interrupted capture/retrieval expose gaps and expired evidence to downstream clients? | [C2](#c2), [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](C5-assistant-harness.md#c5), [C6](C6-observability-and-replay.md#c6); [IC02](../contracts/register.md#ic02), [IC09](../contracts/register.md#ic09), [IC10](../contracts/register.md#ic10) | Discuss reconnect and stale-reference cases using labeled recordings; no transport policy chosen. |
