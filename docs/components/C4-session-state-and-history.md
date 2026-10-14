# Session state and history

## C4

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c4--session-state-and-history); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail is comparatively developed around validation and correction ownership. State schemas, goal representation, conflict rules, and derived-belief invalidation are still missing.

### Supplied responsibilities and behavior

**Purpose:** Maintain the authoritative record of the system's knowledge, beliefs, and commitments during a cook.

**Owns:** Session lifecycle, goal/plan representation, vessel identities, event history, current state, evidence provenance, uncertainty, corrections, and recorded timers or other commitments.

**Receives:** State queries through [IC04](../contracts/register.md#ic04) and proposed mutations from [C5](C5-assistant-harness.md#c5) through [IC05](../contracts/register.md#ic05).

**Provides:** Versioned state/history views through [IC04](../contracts/register.md#ic04) and mutation results through [IC05](../contracts/register.md#ic05); telemetry through [IC09](../contracts/register.md#ic09).

**Boundary:** Authoritative means authoritative for the system's recorded understanding, not guaranteed physical truth. Observations, user statements, inferences, and executed actions remain distinguishable. [C5](C5-assistant-harness.md#c5) owns scheduling and execution; [C4](#c4) owns their recorded status.

**Failure behavior:** Reject invalid updates, stale conflicting changes, and duplicate actions according to the mutation contract. Preserve corrections and their effects in history. Missing information remains unknown.

**Change impact:** State-field meanings or lifecycle changes affect [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](C5-assistant-harness.md#c5), [C7](C7-app-and-interaction-layer.md#c7), and replay. Corrections may invalidate derived beliefs or actions; the ownership of that reconciliation is described under [IC05](../contracts/register.md#ic05).

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| serves queries | [IC04](../contracts/register.md#ic04) | Revisioned state/history to [C3](C3-observation-selection-and-optional-perception.md#c3)/[C5](C5-assistant-harness.md#c5)/[C7](C7-app-and-interaction-layer.md#c7). |
| consumes / returns commit results | [IC05](../contracts/register.md#ic05) | Validate [C5](C5-assistant-harness.md#c5) proposals; record corrections and commitments. |
| produces | [IC09](../contracts/register.md#ic09) | State/commit telemetry. |
| affected by | [O07](../decisions/README.md#o07) | Derived-belief and commitment reconciliation rules. |
| evaluated by | [E04](../experiments/README.md#e04) | Correction, duplicate, and stale-update scenarios. |

### Implementation location and open decisions

**Provisional location:** [backend](../../apps/backend/README.md); physical host remains open. Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O01](../decisions/README.md#o01), [O02](../decisions/README.md#o02), [O07](../decisions/README.md#o07). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | How are goals represented and corrected beliefs invalidated? [O02](../decisions/README.md#o02) / [O07](../decisions/README.md#o07). | [C4](#c4), [C5](C5-assistant-harness.md#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06) | Complete target workflows and wrong-addition/delayed-detection cases ([E05](../experiments/README.md#e05) / [E04](../experiments/README.md#e04)). |
| Source-backed | Where does state execute and what survives connectivity loss? [O01](../decisions/README.md#o01). | [C2](C2-capture-and-observation-management.md#c2), [C4](#c4), [C5](C5-assistant-harness.md#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05) | Target hardware and explicit offline requirements ([E07](../experiments/README.md#e07)). |
| New proposed discovery | Which observations, user statements, inferences, and commitments need visible relationships for correction review? | [C4](#c4), [C5](C5-assistant-harness.md#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05) | Walk a correction through recorded provenance and app display ([E04](../experiments/README.md#e04) / [E05](../experiments/README.md#e05)); no schema selected. |
