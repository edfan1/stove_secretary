# Proposed experiments

These Exx entries are bootstrap proposals derived from source questions and evaluation needs, not previously accepted protocols. Hypotheses/questions are not established facts. All studies are unperformed, owners unassigned, and results absent. Refine methods and recording policy before collecting data; follow [evaluation data boundaries](../../evaluation/README.md). Use the [experiment template](../templates/experiment.md) for a distinct study record; retain IDs and link superseding records.

Shared provenance: [architecture open decisions](../sources/architecture-v0.1.md#7-open-decisions) and [pitch development/evaluation](../sources/pitch.md#development-steps-and-decision-gates). Methods below include explicitly proposed discovery and semantic checks. They do not exhaust requirement coverage or resolve decisions in advance.

## E01

**Investigation:** Placement and capture feasibility. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** Can a candidate hood mount obtain usable one-vessel evidence under cooking conditions?

**Proposed method:** Inspect target hood/view, mounting/removal, heat/cleaning and power constraints; compare sample views under steam, glare, occlusion, and repositioning. Review timestamp/calibration metadata without inventing tolerances.

**Related components:** [C1](../components/C1-physical-sensing-module.md#c1), [C2](../components/C2-capture-and-observation-management.md#c2), [C3](../components/C3-observation-selection-and-optional-perception.md#c3). **Contracts:** [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02).

**Informs decisions:** [O01](../decisions/README.md#o01), [O05](../decisions/README.md#o05). **Requirement links:** [R05](../requirements.md#r05), [R07](../requirements.md#r07).

**Provenance / maturity:** Pitch development stages 1–2; architecture [C1](../components/C1-physical-sensing-module.md#c1)/[C2](../components/C2-capture-and-observation-management.md#c2). Proposed placement method elaborates source feasibility work.

## E02

**Investigation:** Temporal evidence and selection. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** What evidence intervals and selection policy reveal events missed or hidden in individual frames?

**Proposed method:** Use consented labeled recordings with onset annotations, missed/occluded additions and boil/simmer transitions. Compare candidate periodic/triggered selection, retrieval gaps, capture times, repeat selections, and evidence expiry; optional detectors are comparison candidates, not prerequisites.

**Related components:** [C2](../components/C2-capture-and-observation-management.md#c2), [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5), [C6](../components/C6-observability-and-replay.md#c6). **Contracts:** [IC02](../contracts/register.md#ic02), [IC03](../contracts/register.md#ic03), [IC08](../contracts/register.md#ic08), [IC10](../contracts/register.md#ic10).

**Informs decisions:** [O03](../decisions/README.md#o03), [O08](../decisions/README.md#o08). **Requirement links:** [R03](../requirements.md#r03), [R08](../requirements.md#r08), [R13](../requirements.md#r13).

**Provenance / maturity:** Architecture [O03](../decisions/README.md#o03) and change-propagation examples; pitch proposed evaluation metrics.

## E03

**Investigation:** Thermal contribution. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** Does optional thermal improve interpretation on the target cookware beyond RGB alone?

**Proposed method:** Compare RGB-only and thermal-assisted evidence on selected cookware/workflows, labeling events and documenting glare/reflection and alignment limits. Assess trends rather than treating pan readings as food temperature; no sensor choice or expected benefit is fixed.

**Related components:** [C1](../components/C1-physical-sensing-module.md#c1), [C2](../components/C2-capture-and-observation-management.md#c2), [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5). **Contracts:** [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02), [IC03](../contracts/register.md#ic03), [IC11](../contracts/register.md#ic11).

**Informs decisions:** [O05](../decisions/README.md#o05). **Requirement links:** [R01](../requirements.md#r01), [R07](../requirements.md#r07), [R13](../requirements.md#r13).

**Provenance / maturity:** Architecture [O05](../decisions/README.md#o05); pitch background, feasibility gate, and sensor-fusion comparison.

## E04

**Investigation:** Correction and action-lifecycle replay. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** What correction invalidation and timer reconciliation rules preserve coherent recorded state and timely output?

**Proposed method:** Proposed first software investigation: replay normalized evidence and user input in an isolated session through [C4](../components/C4-session-state-and-history.md#c4)/[C5](../components/C5-assistant-harness.md#c5) and a simple client. Review wrong-addition corrections, delayed detections/model responses, stale conflicting revisions, repeated triggers/actions, timer expiry without another model call, and stopped-session output cancellation. Compare history, commitments, and delivery trace against reviewed scenarios. Isolation and adverse-case checks are bootstrap method proposals derived from contract semantics, not settled [O07](../decisions/README.md#o07) rules.

**Related components:** [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C4](../components/C4-session-state-and-history.md#c4), [C5](../components/C5-assistant-harness.md#c5), [C6](../components/C6-observability-and-replay.md#c6), [C7](../components/C7-app-and-interaction-layer.md#c7). **Contracts:** [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07), [IC09](../contracts/register.md#ic09), [IC10](../contracts/register.md#ic10), [IC11](../contracts/register.md#ic11).

**Informs decisions:** [O07](../decisions/README.md#o07). **Requirement links:** [R03](../requirements.md#r03), [R08](../requirements.md#r08), [R09](../requirements.md#r09), [R10](../requirements.md#r10).

**Provenance / maturity:** Architecture [O07](../decisions/README.md#o07), [IC05](../contracts/register.md#ic05), [IC10](../contracts/register.md#ic10), and [IC11](../contracts/register.md#ic11); pitch replay-first stateful-assistant stage.

## E05

**Investigation:** Cooking workflow and interaction prototype. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** Which goal, setup, correction, and recording controls make the assistant understandable and useful?

**Proposed method:** Walk several complete bounded target workflows, including an unstructured goal and clarifications. Use UI sketches/prototypes for setup, session controls, pending/confirmed beliefs, timer corrections, mute, and opt-in recording disclosure. Observe confusion and feedback; distinguish output delivery from cook action. Setup/presentation detail is new proposed discovery; this does not constitute a full privacy audit or live acceptance test.

**Related components:** [C4](../components/C4-session-state-and-history.md#c4), [C5](../components/C5-assistant-harness.md#c5), [C6](../components/C6-observability-and-replay.md#c6), [C7](../components/C7-app-and-interaction-layer.md#c7). **Contracts:** [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07), [IC09](../contracts/register.md#ic09).

**Informs decisions:** [O02](../decisions/README.md#o02), [O06](../decisions/README.md#o06), [O07](../decisions/README.md#o07). **Requirement links:** [R02](../requirements.md#r02), [R04](../requirements.md#r04), [R10](../requirements.md#r10), [R11](../requirements.md#r11).

**Provenance / maturity:** Architecture [O02](../decisions/README.md#o02)/[O06](../decisions/README.md#o06)/[O07](../decisions/README.md#o07); pitch interviews, interface proposal, and usability evaluation.

## E06

**Investigation:** Speech under cooking noise. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** What hardware and recognition/synthesis placement can support useful optional voice?

**Proposed method:** Try candidate microphone positions and voice interactions under hood/cooking noise; inspect recognition failures, turn handling, interruption, output audibility, mute, and delivery feedback. Compare app/device adapter possibilities without selecting a speech stack. If [C1](../components/C1-physical-sensing-module.md#c1) speaker output is explored, identify the separate missing physical-output contract.

**Related components:** [C1](../components/C1-physical-sensing-module.md#c1), [C2](../components/C2-capture-and-observation-management.md#c2), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7). **Contracts:** [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07), [IC11](../contracts/register.md#ic11).

**Informs decisions:** [O04](../decisions/README.md#o04). **Requirement links:** [R10](../requirements.md#r10).

**Provenance / maturity:** Architecture [O04](../decisions/README.md#o04) and [IC01](../contracts/register.md#ic01)/[IC02](../contracts/register.md#ic02)/[IC06](../contracts/register.md#ic06)/[IC07](../contracts/register.md#ic07) open semantics.

## E07

**Investigation:** Disconnected behavior review. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** Where must state/harness execute to support agreed logging, timer, and client behavior during loss of connectivity?

**Proposed method:** Review target hardware and concrete disconnect/reconnect scenarios for capture, evidence retrieval, basic timer expiry, stale app state, and pending delivery. Identify which behavior depends on device, backend, model, or client availability; reconcile the pitch offline proposal with feasible deployment. Failure-scenario detail is proposed discovery, not an accepted offline contract.

**Related components:** [C2](../components/C2-capture-and-observation-management.md#c2), [C4](../components/C4-session-state-and-history.md#c4), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7). **Contracts:** [IC02](../contracts/register.md#ic02), [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07).

**Informs decisions:** [O01](../decisions/README.md#o01). **Requirement links:** [R12](../requirements.md#r12).

**Provenance / maturity:** Architecture [O01](../decisions/README.md#o01) and component failure behavior; pitch data/privacy offline path.

## E08

**Investigation:** Performance and intervention pilot. **Status:** Proposed; not run. **Owner:** Unassigned. **Results:** None.

**Question / hypothesis:** What delay, state accuracy, cost, and interruption/usefulness targets are realistic for bounded cooks?

**Proposed method:** After the slice and evidence setup exist, pilot labeled cooking sessions; measure the metric categories in [R13](../requirements.md#r13), record intervention reasons/grounding and user feedback, and compare a simple timer/recipe-step baseline. Define annotation and review procedures before claiming results. Use measurements to propose targets; no numeric pass thresholds or results exist.

**Related components:** [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5), [C6](../components/C6-observability-and-replay.md#c6), [C7](../components/C7-app-and-interaction-layer.md#c7). **Contracts:** [IC03](../contracts/register.md#ic03), [IC07](../contracts/register.md#ic07), [IC09](../contracts/register.md#ic09), [IC11](../contracts/register.md#ic11).

**Informs decisions:** [O08](../decisions/README.md#o08). **Requirement links:** [R02](../requirements.md#r02), [R09](../requirements.md#r09), [R13](../requirements.md#r13).

**Provenance / maturity:** Architecture [O08](../decisions/README.md#o08); pitch proposed metrics and alert-burden study.
