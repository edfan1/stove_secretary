# Documentation map

Read the [overview](overview.md) first, then use IDs below to reach relevant definitions. [Maintenance](maintenance.md) explains change-impact review. [Sources](sources/README.md) preserve provenance; [templates](templates/decision.md) support new decisions and [experiment records](templates/experiment.md). Application locations are linked from component documents.

The graph consists of canonical definitions and useful links. Status and implementation maturity live at each definition, not in this index. Overview, application READMEs, templates, and source snapshots are supporting material rather than extra architecture nodes. Search stable IDs to find incoming references; no mirrored-backlink register is required.

## Canonical ID index

| ID | Type | Canonical definition |
| --- | --- | --- |
| C1 | Component | [Physical sensing module](components/C1-physical-sensing-module.md#c1) |
| C2 | Component | [Capture and observation management](components/C2-capture-and-observation-management.md#c2) |
| C3 | Component | [Observation selection and optional perception](components/C3-observation-selection-and-optional-perception.md#c3) |
| C4 | Component | [Session state and history](components/C4-session-state-and-history.md#c4) |
| C5 | Component | [Assistant harness](components/C5-assistant-harness.md#c5) |
| C6 | Component | [Observability and replay](components/C6-observability-and-replay.md#c6) |
| C7 | Component | [App and interaction layer](components/C7-app-and-interaction-layer.md#c7) |
| IC01 | Contract | [Device acquisition and configuration](contracts/register.md#ic01) |
| IC02 | Contract | [Normalized observation access](contracts/register.md#ic02) |
| IC03 | Contract | [Selected evidence and candidate events](contracts/register.md#ic03) |
| IC04 | Contract | [Session state/history access](contracts/register.md#ic04) |
| IC05 | Contract | [Session mutation and commit result](contracts/register.md#ic05) |
| IC06 | Contract | [User interaction and feedback](contracts/register.md#ic06) |
| IC07 | Contract | [Presentation and notification request](contracts/register.md#ic07) |
| IC08 | Contract | [Observation priorities](contracts/register.md#ic08) |
| IC09 | Contract | [Instrumentation](contracts/register.md#ic09) |
| IC10 | Contract | [Development replay](contracts/register.md#ic10) |
| IC11 | Contract | [Model interaction](contracts/register.md#ic11) |
| O01 | Decision question | [Where C4/C5 execute and what survives connectivity loss](decisions/README.md#o01) |
| O02 | Decision question | [Initial recipe/goal representation](decisions/README.md#o02) |
| O03 | Decision question | [Sampling strategy and required temporal evidence](decisions/README.md#o03) |
| O04 | Decision question | [Speech hardware and recognition/synthesis location](decisions/README.md#o04) |
| O05 | Decision question | [Whether thermal adds useful information](decisions/README.md#o05) |
| O06 | Decision question | [Retention, consent, and replay dataset policy](decisions/README.md#o06) |
| O07 | Decision question | [Correction invalidation and timer reconciliation rules](decisions/README.md#o07) |
| O08 | Decision question | [Performance and intervention targets](decisions/README.md#o08) |
| R01 | Requirement / scope boundary | [Thermal interpretation limits](requirements.md#r01) |
| R02 | Requirement / scope boundary | [Goal-led assistance](requirements.md#r02) |
| R03 | Requirement / scope boundary | [Bounded core demonstration](requirements.md#r03) |
| R04 | Requirement / scope boundary | [Unstructured session](requirements.md#r04) |
| R05 | Requirement / scope boundary | [First-version bounds](requirements.md#r05) |
| R06 | Requirement / scope boundary | [Assistance prototype limits](requirements.md#r06) |
| R07 | Requirement / scope boundary | [Capture and calibration](requirements.md#r07) |
| R08 | Requirement / scope boundary | [Temporal state and provenance](requirements.md#r08) |
| R09 | Requirement / scope boundary | [Grounded intervention](requirements.md#r09) |
| R10 | Requirement / scope boundary | [Visible state and user control](requirements.md#r10) |
| R11 | Requirement / scope boundary | [Recording and cloud disclosure](requirements.md#r11) |
| R12 | Requirement / scope boundary | [Offline logging and basic timers](requirements.md#r12) |
| R13 | Requirement / scope boundary | [Measured evaluation](requirements.md#r13) |
| E01 | Experiment | [Placement and capture feasibility](experiments/README.md#e01) |
| E02 | Experiment | [Temporal evidence and selection](experiments/README.md#e02) |
| E03 | Experiment | [Thermal contribution](experiments/README.md#e03) |
| E04 | Experiment | [Correction and action-lifecycle replay](experiments/README.md#e04) |
| E05 | Experiment | [Cooking workflow and interaction prototype](experiments/README.md#e05) |
| E06 | Experiment | [Speech under cooking noise](experiments/README.md#e06) |
| E07 | Experiment | [Disconnected behavior review](experiments/README.md#e07) |
| E08 | Experiment | [Performance and intervention pilot](experiments/README.md#e08) |
| D001 | Decision record | [Monorepo](decisions/D001-monorepo.md#d001) |
