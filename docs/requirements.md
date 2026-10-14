# Requirements and scope

IDs below organize source statements; they do not establish new team acceptance. **No requirement is recorded as accepted by this bootstrap.** Source-backed scope boundaries are preserved as boundaries of the supplied concept. Proposed requirements need review and evidence; implementation is absent. IDs follow distinct extracted statements in pitch section order, with risk detail attached to its relevant requirement. Architecture semantics take precedence for component responsibilities.

The pitch's detector-first mechanism is historical; behavior/evaluation below may be achieved through model-centered interpretation with optional perception. See [source reconciliation](overview.md#source-reconciliation). Proposed experiments are plans, not test results or complete acceptance coverage.

## R01

**Meaning:** Thermal interpretation limits. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Treat optional thermal as a validated trend; avoid equating apparent pan infrared temperature with food temperature.

**Source:** [Pitch: background and prior work](sources/pitch.md#background-and-prior-work). **Relevant components:** [C1](components/C1-physical-sensing-module.md#c1), [C2](components/C2-capture-and-observation-management.md#c2), [C3](components/C3-observation-selection-and-optional-perception.md#c3), [C5](components/C5-assistant-harness.md#c5).

**Proposed evaluation:** [E03](experiments/README.md#e03). **Coverage / unresolved detail:** Specific calibration/acceptance thresholds remain open.

## R02

**Meaning:** Goal-led assistance. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Allow a plain-language dish/goal, questions, and corrections; maintain timestamped context and offer suggestions when there is a reason to act.

**Source:** [Pitch: proposed experience and scope](sources/pitch.md#proposed-experience-and-scope). **Relevant components:** [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E05](experiments/README.md#e05). **Coverage / unresolved detail:** Useful intervention criteria also need pilot evidence ([E08](experiments/README.md#e08)).

## R03

**Meaning:** Bounded core demonstration. **Status:** Proposed requirement. **Implementation:** Unimplemented.

In a sauce or pasta workflow, identify boil/simmer and coarse addition, start or suggest a timer, answer a visual question using recent history, and provide a timely reminder.

**Source:** [Pitch: proposed experience and scope](sources/pitch.md#proposed-experience-and-scope). **Relevant components:** [C2](components/C2-capture-and-observation-management.md#c2), [C3](components/C3-observation-selection-and-optional-perception.md#c3), [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E04](experiments/README.md#e04). **Coverage / unresolved detail:** Proposed replay investigates state/action scenarios; full live demonstration coverage remains to be designed with [E02](experiments/README.md#e02) and [E08](experiments/README.md#e08).

## R04

**Meaning:** Unstructured session. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Demonstrate at least one session without loading a machine-readable recipe.

**Source:** [Pitch: proposed experience and scope](sources/pitch.md#proposed-experience-and-scope). **Relevant components:** [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E05](experiments/README.md#e05). **Coverage / unresolved detail:** Workflow investigation informs [O02](decisions/README.md#o02); a full unstructured live acceptance case is not yet specified.

## R05

**Meaning:** First-version bounds. **Status:** Source-backed scope boundary. **Implementation:** Unimplemented.

One cooktop, fixed overhead camera, one active vessel, and two repeatable dishes proposed by the pitch; pasta and tomato sauce are examples. Extra vessels, pan-frying, ventilation recommendations, and richer ingredient recognition are extensions after reliability.

**Source:** [Pitch: proposed experience and scope](sources/pitch.md#proposed-experience-and-scope). **Relevant components:** [C1](components/C1-physical-sensing-module.md#c1), [C3](components/C3-observation-selection-and-optional-perception.md#c3), [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E01](experiments/README.md#e01). **Coverage / unresolved detail:** Placement feasibility only; full scope-conformance evaluation is missing and actual dishes remain to be selected. [Architecture section 1](sources/architecture-v0.1.md#1-scope-and-decision-status) retains the one-vessel bounded scope.

## R06

**Meaning:** Assistance prototype limits. **Status:** Source-backed scope boundary. **Implementation:** Unimplemented.

Do not replace ventilation, control gas/heat, alter mains wiring, or present the prototype as a certified fire/food-safety monitor.

**Source:** [Pitch: proposed experience and scope](sources/pitch.md#proposed-experience-and-scope). **Relevant components:** [C1](components/C1-physical-sensing-module.md#c1), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** **Missing:** no proposed experiment linked. **Coverage / unresolved detail:** No dedicated engineering/UI scope audit is defined yet.

## R07

**Meaning:** Capture and calibration. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Use a removable heat-conscious RGB mount, local timestamps, and calibration of image regions; optional thermal alignment is conditional on feasibility.

**Source:** [Pitch: technical approach](sources/pitch.md#technical-approach). **Relevant components:** [C1](components/C1-physical-sensing-module.md#c1), [C2](components/C2-capture-and-observation-management.md#c2).

**Proposed evaluation:** [E01](experiments/README.md#e01). **Coverage / unresolved detail:** Concrete mount/calibration procedure and timestamp precision are missing; [IC01](contracts/register.md#ic01)/[IC02](contracts/register.md#ic02) preserve evidence semantics.

## R08

**Meaning:** Temporal state and provenance. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Keep intent, observations, inferred state, timers, and corrections with temporal evidence and distinguishable provenance. Unknown remains valid; isolated poor frames should not establish unsupported state.

**Source:** [Pitch: technical approach](sources/pitch.md#technical-approach). **Relevant components:** [C2](components/C2-capture-and-observation-management.md#c2), [C3](components/C3-observation-selection-and-optional-perception.md#c3), [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5).

**Proposed evaluation:** [E04](experiments/README.md#e04). **Coverage / unresolved detail:** Temporal evidence selection also needs [E02](experiments/README.md#e02); model confidence is not assumed calibrated under the architecture conventions.

## R09

**Meaning:** Grounded intervention. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Check grounding, uncertainty, urgency, repetition, and whether the cook already acted. Use conservative wording or clarification for high-consequence claims; avoid unsupported doneness/internal-temperature claims.

**Source:** [Pitch: technical approach](sources/pitch.md#technical-approach). **Relevant components:** [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E08](experiments/README.md#e08). **Coverage / unresolved detail:** [E04](experiments/README.md#e04) can review unsupported actions; no finalized policy thresholds or comprehensive high-consequence review procedure exist. Also sourced from [Main risks and design responses](sources/pitch.md#main-risks-and-design-responses).

## R10

**Meaning:** Visible state and user control. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Show beliefs and their basis; allow timer/state corrections and usefulness feedback, with accessible mute/privacy controls. Voice is optional.

**Source:** [Pitch: technical approach](sources/pitch.md#technical-approach). **Relevant components:** [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E05](experiments/README.md#e05). **Coverage / unresolved detail:** Background delivery and detailed voice controls remain design gaps; delivery does not prove user action.

## R11

**Meaning:** Recording and cloud disclosure. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Process continuous video locally where practical; propose sending only selected images/compact state to a cloud model, disclosing this in the UI, and making session recording opt-in.

**Source:** [Pitch: technical approach](sources/pitch.md#technical-approach). **Relevant components:** [C2](components/C2-capture-and-observation-management.md#c2), [C5](components/C5-assistant-harness.md#c5), [C6](components/C6-observability-and-replay.md#c6), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E05](experiments/README.md#e05). **Coverage / unresolved detail:** Usability review covers controls only; a complete data-flow/retention audit is not designed. [O06](decisions/README.md#o06) remains open.

## R12

**Meaning:** Offline logging and basic timers. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Keep an offline path for sensor logging and basic timers when connectivity drops, as proposed in the pitch.

**Source:** [Pitch: technical approach](sources/pitch.md#technical-approach). **Relevant components:** [C2](components/C2-capture-and-observation-management.md#c2), [C4](components/C4-session-state-and-history.md#c4), [C5](components/C5-assistant-harness.md#c5), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E07](experiments/README.md#e07). **Coverage / unresolved detail:** Architecture leaves deployment and survivable behavior under [O01](decisions/README.md#o01) unresolved; no offline guarantee is implemented.

## R13

**Meaning:** Measured evaluation. **Status:** Proposed requirement. **Implementation:** Unimplemented.

Measure event precision/recall, labeled-onset delay, timer-start accuracy, response grounding, false prompts per cooking hour, response latency, and user-rated usefulness. Compare with a timer/recipe-step baseline and RGB-only versus fusion where applicable.

**Source:** [Pitch: development steps and decision gates](sources/pitch.md#development-steps-and-decision-gates). **Relevant components:** [C3](components/C3-observation-selection-and-optional-perception.md#c3), [C5](components/C5-assistant-harness.md#c5), [C6](components/C6-observability-and-replay.md#c6), [C7](components/C7-app-and-interaction-layer.md#c7).

**Proposed evaluation:** [E08](experiments/README.md#e08). **Coverage / unresolved detail:** [E02](experiments/README.md#e02)/[E03](experiments/README.md#e03)/[E04](experiments/README.md#e04)/[E05](experiments/README.md#e05) cover parts of the plan; held-out splits, labels, and full end-to-end evaluation remain undefined. Numerical targets follow pilot baselines.

## Proposed targets

Metric categories are preserved in R13; no new numerical performance targets, results, or thresholds are supplied. The pitch proposes a bounded number of dishes and a demonstration session, which are scope/demo statements rather than measured performance. Resolve [O08](decisions/README.md#o08) with pilot evidence before selecting targets.
