# Observability and replay

## C6

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c6--observability-and-replay); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail covers correlation and replay isolation. Concrete trace formats, replay clock, dependency substitution, retention, and redaction remain unspecified.

### Supplied responsibilities and behavior

**Purpose:** Make behavior explainable and support repeatable development experiments.

**Owns:** Correlated telemetry, session traces, evaluation exports, replay records, and development replay entry points. Operational telemetry and opt-in retained cooking evidence have separate retention decisions.

**Receives:** Instrumentation from the other components through [IC09](../contracts/register.md#ic09).

**Provides:** Diagnostic and evaluation views; development replay inputs through [IC10](../contracts/register.md#ic10).

**Boundary:** Does not act as a second source of live session state. Replay preserves recorded inputs and timing; derived outputs are recomputed for comparison unless an explicitly recorded dependency is being replayed.

**Failure behavior:** Missing telemetry should be visible. A telemetry failure should not unnecessarily block assistance. Replay must isolate timers and notifications from the real live session.

**Change impact:** Trace or replay format changes affect instrumentation across components and comparability with previous experiments.

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| consumes | [IC09](../contracts/register.md#ic09) | Instrumentation from [C1](C1-physical-sensing-module.md#c1)–[C5](C5-assistant-harness.md#c5) and [C7](C7-app-and-interaction-layer.md#c7). |
| produces | [IC10](../contracts/register.md#ic10) | Development inputs for [C3](C3-observation-selection-and-optional-perception.md#c3)/[C5](C5-assistant-harness.md#c5). |
| affected by | [O06](../decisions/README.md#o06) | Retention and evidence availability. |
| evaluated by | [E04](../experiments/README.md#e04) | Isolation and trace provenance in replay. |

### Implementation location and open decisions

**Provisional location:** instrumentation across applications and replay/evaluation entry points under [evaluation](../../evaluation/README.md). Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O06](../decisions/README.md#o06), [O08](../decisions/README.md#o08). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | What retention/consent and target metrics permit useful replay? [O06](../decisions/README.md#o06) / [O08](../decisions/README.md#o08). | [C2](C2-capture-and-observation-management.md#c2), [C6](#c6), [C7](C7-app-and-interaction-layer.md#c7); [IC09](../contracts/register.md#ic09), [IC10](../contracts/register.md#ic10) | Recording-control review and pilot measurements ([E05](../experiments/README.md#e05) / [E08](../experiments/README.md#e08)). |
| Source-backed | What replay clock and recorded-versus-live dependency substitution support comparison? | [C3](C3-observation-selection-and-optional-perception.md#c3), [C5](C5-assistant-harness.md#c5), [C6](#c6); [IC10](../contracts/register.md#ic10), [IC11](../contracts/register.md#ic11) | Replay case review including timer isolation and dependency versions ([E04](../experiments/README.md#e04)); exact format remains open. |
| New proposed discovery | What should a review report when media or telemetry is incomplete? | [C2](C2-capture-and-observation-management.md#c2), [C5](C5-assistant-harness.md#c5), [C6](#c6); [IC02](../contracts/register.md#ic02), [IC09](../contracts/register.md#ic09), [IC10](../contracts/register.md#ic10) | Inspect missing-evidence cases and mark comparison limits instead of filling gaps. |
