# Decisions and unresolved choices

Accepted decisions have explicit authorization or recorded agreement; proposed decisions await agreement; open decisions remain unresolved; superseded decisions retain rationale and link replacements. Implemented choices are separately reported and do not automatically become accepted. Record new substantive choices using the [decision template](../templates/decision.md).

[D001: monorepo](D001-monorepo.md#d001) records the accepted repository choice. O01–O08 below retain the architecture's open identities and next useful evidence. Keep an Oxx section when resolving it and link its resolution record.

**Provenance:** [Architecture section 7](../sources/architecture-v0.1.md#7-open-decisions). Affected components and next evidence come from that source. Contract links and proposed experiment links are bootstrap impact-navigation additions, not accepted solutions. Decision owners were not assigned in the source.

## O01

**Question:** Where C4/C5 execute and what survives connectivity loss

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C2](../components/C2-capture-and-observation-management.md#c2), [C4](../components/C4-session-state-and-history.md#c4), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7). **Related contracts to review:** [IC02](../contracts/register.md#ic02), [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC07](../contracts/register.md#ic07).

**Next useful evidence (source):** Target hardware and offline requirements. **Proposed investigation:** [E07](../experiments/README.md#e07). No evidence/results supplied.

## O02

**Question:** Initial recipe/goal representation

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C4](../components/C4-session-state-and-history.md#c4), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7). **Related contracts to review:** [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06), [IC11](../contracts/register.md#ic11).

**Next useful evidence (source):** A few complete target workflows and clarification examples. **Proposed investigation:** [E05](../experiments/README.md#e05). No evidence/results supplied.

## O03

**Question:** Sampling strategy and required temporal evidence

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C2](../components/C2-capture-and-observation-management.md#c2), [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5). **Related contracts to review:** [IC02](../contracts/register.md#ic02), [IC03](../contracts/register.md#ic03), [IC08](../contracts/register.md#ic08).

**Next useful evidence (source):** Labeled recordings with missed/occluded events. **Proposed investigation:** [E02](../experiments/README.md#e02). No evidence/results supplied.

## O04

**Question:** Speech hardware and recognition/synthesis location

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C1](../components/C1-physical-sensing-module.md#c1), [C2](../components/C2-capture-and-observation-management.md#c2), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7). **Related contracts to review:** [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07), [IC11](../contracts/register.md#ic11).

**Next useful evidence (source):** Voice prototype under cooking/hood noise. **Proposed investigation:** [E06](../experiments/README.md#e06). No evidence/results supplied.

## O05

**Question:** Whether thermal adds useful information

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C1](../components/C1-physical-sensing-module.md#c1), [C2](../components/C2-capture-and-observation-management.md#c2), [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5). **Related contracts to review:** [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02), [IC03](../contracts/register.md#ic03), [IC11](../contracts/register.md#ic11).

**Next useful evidence (source):** RGB-only versus thermal-assisted trials on target cookware. **Proposed investigation:** [E03](../experiments/README.md#e03). No evidence/results supplied.

## O06

**Question:** Retention, consent, and replay dataset policy

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C2](../components/C2-capture-and-observation-management.md#c2), [C6](../components/C6-observability-and-replay.md#c6), [C7](../components/C7-app-and-interaction-layer.md#c7). **Related contracts to review:** [IC02](../contracts/register.md#ic02), [IC06](../contracts/register.md#ic06), [IC09](../contracts/register.md#ic09), [IC10](../contracts/register.md#ic10).

**Next useful evidence (source):** Recording controls and evaluation needs. **Proposed investigation:** [E05](../experiments/README.md#e05). No evidence/results supplied.

## O07

**Question:** Correction invalidation and timer reconciliation rules

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C4](../components/C4-session-state-and-history.md#c4), [C5](../components/C5-assistant-harness.md#c5), [C7](../components/C7-app-and-interaction-layer.md#c7). **Related contracts to review:** [IC04](../contracts/register.md#ic04), [IC05](../contracts/register.md#ic05), [IC06](../contracts/register.md#ic06), [IC07](../contracts/register.md#ic07), [IC11](../contracts/register.md#ic11).

**Next useful evidence (source):** Wrong-addition, delayed-detection, and duplicate-trigger cases. **Proposed investigation:** [E04](../experiments/README.md#e04). No evidence/results supplied.

## O08

**Question:** Performance and intervention targets

**Status:** Open. **Owner:** Unassigned.

**Affected components:** [C3](../components/C3-observation-selection-and-optional-perception.md#c3), [C5](../components/C5-assistant-harness.md#c5), [C6](../components/C6-observability-and-replay.md#c6). **Related contracts to review:** [IC03](../contracts/register.md#ic03), [IC07](../contracts/register.md#ic07), [IC09](../contracts/register.md#ic09), [IC11](../contracts/register.md#ic11).

**Next useful evidence (source):** Pilot measurements of delay, state accuracy, and prompt usefulness. **Proposed investigation:** [E08](../experiments/README.md#e08). No evidence/results supplied.
