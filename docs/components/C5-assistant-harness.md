# Assistant harness

## C5

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c5--assistant-harness); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail is comparatively developed around orchestration and validation. Tool vocabulary, concurrency, resource budgets, policy thresholds, and provider/deployment choices remain unspecified.

### Supplied responsibilities and behavior

**Purpose:** Coordinate reasoning and assistance across observations, state, user input, and scheduled events.

**Owns:** Trigger handling, context assembly, model adapters, tool exposure/execution, scheduling, intervention policy, and routing outputs. Optional speech recognition and synthesis adapters live here provisionally.

**Receives:** Selected observations through [IC03](../contracts/register.md#ic03); evidence through [IC02](../contracts/register.md#ic02); state through [IC04](../contracts/register.md#ic04); user interactions through [IC06](../contracts/register.md#ic06); model responses through [IC11](../contracts/register.md#ic11); replay inputs through [IC10](../contracts/register.md#ic10). Scheduled triggers originate within the harness.

**Provides:** State mutations through [IC05](../contracts/register.md#ic05); analysis priorities through [IC08](../contracts/register.md#ic08); presentation requests through [IC07](../contracts/register.md#ic07); model requests through [IC11](../contracts/register.md#ic11); telemetry through [IC09](../contracts/register.md#ic09).

**Internal responsibilities:**

- Resolve triggers and assemble relevant evidence and state.
- Request model interpretation and validate proposed actions.
- Execute tools with identifiable, duplicate-resistant actions.
- Maintain observation checkpoints and user timers.
- Decide whether an intervention is grounded, timely, and worth an interruption.
- Handle corrections, session stop, and cancellation of obsolete work.

**Boundary:** Models propose interpretations and actions; backend logic applies validated changes and executes them. An established timer can expire without another model call.

**Failure behavior:** Handle model timeouts, invalid responses, stale reasoning, and delivery failures explicitly. Recheck relevant state before applying delayed results. Preserve local/basic timer behavior where supported during model unavailability; exact deployment determines the failure boundary.

**Change impact:** Tools, scheduling, or intervention changes can affect [C4](C4-session-state-and-history.md#c4), [C7](C7-app-and-interaction-layer.md#c7), [C3](C3-observation-selection-and-optional-perception.md#c3), provider adapters, and evaluation. A prompt-only change may alter externally visible behavior and belongs in the change record.

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| queries | [IC02](../contracts/register.md#ic02) | Evidence retrieval; audio can bypass [C3](C3-observation-selection-and-optional-perception.md#c3). |
| consumes | [IC03](../contracts/register.md#ic03) | Selected evidence and candidates. |
| queries | [IC04](../contracts/register.md#ic04) | State used for reasoning and validation. |
| mutates through | [IC05](../contracts/register.md#ic05) | [C4](C4-session-state-and-history.md#c4) validates authoritative changes. |
| consumes / returns acceptance | [IC06](../contracts/register.md#ic06) | Interactions and delivery feedback from [C7](C7-app-and-interaction-layer.md#c7). |
| produces | [IC07](../contracts/register.md#ic07) | Presentation requests to [C7](C7-app-and-interaction-layer.md#c7). |
| produces / receives acceptance | [IC08](../contracts/register.md#ic08) | Priorities to [C3](C3-observation-selection-and-optional-perception.md#c3). |
| produces | [IC09](../contracts/register.md#ic09) | Correlated reasoning/execution telemetry. |
| consumes | [IC10](../contracts/register.md#ic10) | Isolated replay triggers from [C6](C6-observability-and-replay.md#c6). |
| queries / consumes proposals | [IC11](../contracts/register.md#ic11) | External model via validated adapter. |
| evaluated by | [E04](../experiments/README.md#e04) | Reconciliation and action lifecycle. |

### Implementation location and open decisions

**Provisional location:** [backend](../../apps/backend/README.md); physical host and speech placement remain open. Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O01](../decisions/README.md#o01), [O02](../decisions/README.md#o02), [O03](../decisions/README.md#o03), [O04](../decisions/README.md#o04), [O05](../decisions/README.md#o05), [O07](../decisions/README.md#o07), [O08](../decisions/README.md#o08). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | How should corrections reconcile pending work and timers? [O07](../decisions/README.md#o07); how should goals guide reasoning? [O02](../decisions/README.md#o02). | [C4](C4-session-state-and-history.md#c4), [C5](#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC04](../contracts/register.md#ic04)–[IC07](../contracts/register.md#ic07), [IC11](../contracts/register.md#ic11) | Wrong-addition, delayed-response, and duplicate-trigger replay ([E04](../experiments/README.md#e04)), plus workflow discussion ([E05](../experiments/README.md#e05)). |
| Source-backed | What deployment, speech, cadence, and intervention choices work? [O01](../decisions/README.md#o01) / [O04](../decisions/README.md#o04) / [O03](../decisions/README.md#o03) / [O08](../decisions/README.md#o08). | [C2](C2-capture-and-observation-management.md#c2), [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](#c5), [C6](C6-observability-and-replay.md#c6), [C7](C7-app-and-interaction-layer.md#c7); [IC02](../contracts/register.md#ic02), [IC03](../contracts/register.md#ic03), [IC07](../contracts/register.md#ic07), [IC08](../contracts/register.md#ic08), [IC11](../contracts/register.md#ic11) | Offline, noisy voice, temporal evidence, and pilot trials ([E07](../experiments/README.md#e07) / [E06](../experiments/README.md#e06) / [E02](../experiments/README.md#e02) / [E08](../experiments/README.md#e08)). |
| New proposed discovery | How should overlapping triggers and delayed responses be reviewed against newly corrected state? | [C4](C4-session-state-and-history.md#c4), [C5](#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC05](../contracts/register.md#ic05), [IC07](../contracts/register.md#ic07), [IC11](../contracts/register.md#ic11) | Inspect stale-revision and correction races in replay ([E04](../experiments/README.md#e04)); no concurrency policy selected. |
