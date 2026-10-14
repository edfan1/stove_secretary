# Physical sensing module

## C1

**Design status:** Working baseline from [architecture section 2](../sources/architecture-v0.1.md#c1--physical-sensing-module); contract behavior remains proposed. **Implementation status:** Unimplemented; repository contains planning/README placeholders only.

Supplied detail covers acquisition boundaries and basic health behavior. Physical design, parts selection, placement, and provisioning are not specified.

### Supplied responsibilities and behavior

**Purpose:** Obtain usable camera, thermal, and potentially microphone signals from the cooking environment.

**Owns:** Sensors, mounting, enclosure, power, device drivers, basic device configuration, and hardware health reporting. Local computing hardware may host other components without absorbing their logical responsibilities.

**Receives:** Physical signals and capture/device configuration through [IC01](../contracts/register.md#ic01).

**Provides:** Device samples and health information to [C2](C2-capture-and-observation-management.md#c2) through [IC01](../contracts/register.md#ic01). Device telemetry may reach [C6](C6-observability-and-replay.md#c6) through the capture software under [IC09](../contracts/register.md#ic09).

**Boundary:** Semantic cooking interpretation belongs downstream. The camera being operational does not establish that the vessel is visible or that the scene is interpretable.

**Failure behavior:** Report unavailable or failed sensors explicitly. RGB-only operation remains possible when thermal hardware is absent. Preserve the distinction between an optional sensor and one that unexpectedly disconnected.

**Change impact:** Sensor format, calibration, orientation, or timing changes primarily affect [C2](C2-capture-and-observation-management.md#c2) and may alter [C3](C3-observation-selection-and-optional-perception.md#c3)'s evidence quality. Mounting changes can invalidate prior evaluation results even when software contracts are unchanged.

See [shared contract conventions](../contracts/register.md#shared-conventions) for identity, time, uncertainty, revision, and lifecycle semantics. Responsibilities above are supplied design, not implemented guarantees.

### Relationships

The register owns participant/direction details; this table summarizes use and proposed investigation.

| Relationship | Target | Reason |
| --- | --- | --- |
| produces / receives configuration | [IC01](../contracts/register.md#ic01) | Device samples/health to [C2](C2-capture-and-observation-management.md#c2); configuration from [C2](C2-capture-and-observation-management.md#c2). |
| produces | [IC09](../contracts/register.md#ic09) | Device telemetry may pass through capture software to [C6](C6-observability-and-replay.md#c6). |
| evaluated by | [E01](../experiments/README.md#e01) | Placement and acquisition feasibility. |
| evaluated by | [E03](../experiments/README.md#e03) | Optional thermal value. |

### Implementation location and open decisions

**Provisional location:** [hardware](../../hardware/README.md) and acquisition software in [device](../../apps/device/README.md). Folder allocation does not settle execution host, technology, or offline behavior.

Source-backed open decisions: [O04](../decisions/README.md#o04), [O05](../decisions/README.md#o05). Decision owners are unassigned where the source gives none. Additional contract-level questions remain in the linked register.

### Design gaps

Source-backed questions come first. New proposed discovery questions are not accepted requirements, assigned work, or settled answers. Relationships below identify possible impact to investigate.

| Provenance | Question | May affect | Next useful evidence / discussion |
| --- | --- | --- | --- |
| Source-backed | Which speech hardware and thermal sensors help? [O04](../decisions/README.md#o04) / [O05](../decisions/README.md#o05). | [C1](#c1), [C2](C2-capture-and-observation-management.md#c2), [C5](C5-assistant-harness.md#c5), [C7](C7-app-and-interaction-layer.md#c7); [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02) | Cooking/hood-noise prototype and RGB-only versus thermal cookware trials ([E06](../experiments/README.md#e06) / [E03](../experiments/README.md#e03)). |
| Pitch-backed discovery | What field of view, mount, heat/cleaning limits, and power arrangement fit the target hood? | [C1](#c1), [C2](C2-capture-and-observation-management.md#c2), [C3](C3-observation-selection-and-optional-perception.md#c3); [IC01](../contracts/register.md#ic01), [IC02](../contracts/register.md#ic02) | Inspect target hood; placement and occlusion trials with a removable mount ([E01](../experiments/README.md#e01)). |
| New proposed discovery | How will initial device provisioning, orientation, and reconnect configuration work? | [C1](#c1), [C2](C2-capture-and-observation-management.md#c2), [C7](C7-app-and-interaction-layer.md#c7); [IC01](../contracts/register.md#ic01); [O01](../decisions/README.md#o01) | Walk through first setup and reconnect with candidate hardware; review cross-layer responsibilities. |
