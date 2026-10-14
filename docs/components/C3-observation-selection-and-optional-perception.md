# Observation selection and optional perception

## C3

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c3--observation-selection-and-optional-perception); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail covers selection boundaries and replaceable perception. Cadence, candidate formats, crop policy, and acceptance criteria are not specified.

### Supplied responsibilities and behavior

**Purpose:** Determine which evidence deserves analysis and optionally extract inexpensive candidate events.

**Owns:** Sampling policies, frame/clip selection, vessel-region crops, local motion/change processing, and any dedicated detectors. Candidate events remain hypotheses until accepted into state.

**Receives:** Observations from [C2](C2-capture-and-observation-management.md#c2) through [IC02](../contracts/register.md#ic02); state views from [C4](C4-session-state-and-history.md#c4) through [IC04](../contracts/register.md#ic04); analysis priorities from [C5](C5-assistant-harness.md#c5) through [IC08](../contracts/register.md#ic08). Replay can supply equivalent observations through [IC10](../contracts/register.md#ic10).

**Provides:** Selected evidence and optional candidate events to [C5](C5-assistant-harness.md#c5) through [IC03](../contracts/register.md#ic03); telemetry through [IC09](../contracts/register.md#ic09).

**Boundary:** Does not directly modify authoritative session state or issue cooking advice. Local perception is replaceable; the system does not depend on a fixed catalog of specialized detectors.

**Failure behavior:** Include visibility limitations and missing evidence. A missing detection is not proof that an action did not occur. Periodic observation provides a path for discovering changes missed by event triggers.

**Change impact:** Selection or detector changes affect [C5](C5-assistant-harness.md#c5)'s available evidence, state accuracy, prompt timing, and evaluation. A compatible payload can still materially change behavior and cost.

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| queries | [IC02](../contracts/register.md#ic02) | Fresh and buffered evidence from [C2](C2-capture-and-observation-management.md#c2). |
| queries | [IC04](../contracts/register.md#ic04) | Revisioned context from [C4](C4-session-state-and-history.md#c4). |
| produces | [IC03](../contracts/register.md#ic03) | Selections and optional hypotheses for [C5](C5-assistant-harness.md#c5). |
| consumes / returns acceptance | [IC08](../contracts/register.md#ic08) | [C5](C5-assistant-harness.md#c5) priorities bounded by capability. |
| consumes | [IC10](../contracts/register.md#ic10) | Equivalent development observations from [C6](C6-observability-and-replay.md#c6). |
| produces | [IC09](../contracts/register.md#ic09) | Selection telemetry. |
| evaluated by | [E02](../experiments/README.md#e02) | Missed/occluded events and sampling tradeoffs. |

### Implementation location and open decisions

**Provisional location:** [device](../../apps/device/README.md) and/or [backend](../../apps/backend/README.md), pending experiments. Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O03](../decisions/README.md#o03), [O05](../decisions/README.md#o05), [O08](../decisions/README.md#o08). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | Which sampling and temporal evidence strategy works? [O03](../decisions/README.md#o03); is thermal useful? [O05](../decisions/README.md#o05); what targets are realistic? [O08](../decisions/README.md#o08). | [C2](C2-capture-and-observation-management.md#c2), [C3](#c3), [C5](C5-assistant-harness.md#c5), [C6](C6-observability-and-replay.md#c6); [IC02](../contracts/register.md#ic02), [IC03](../contracts/register.md#ic03), [IC08](../contracts/register.md#ic08) | Missed/occluded-event recordings, thermal comparison, and pilot metrics ([E02](../experiments/README.md#e02) / [E03](../experiments/README.md#e03) / [E08](../experiments/README.md#e08)). |
| New proposed discovery | Which visibility limitations and repeated candidates should prompt clarification rather than another selection? | [C3](#c3), [C4](C4-session-state-and-history.md#c4), [C5](C5-assistant-harness.md#c5); [IC03](../contracts/register.md#ic03), [IC04](../contracts/register.md#ic04), [IC08](../contracts/register.md#ic08) | Review occlusion and duplicate-trigger examples across temporal selection and replay ([E02](../experiments/README.md#e02) / [E04](../experiments/README.md#e04)). |
