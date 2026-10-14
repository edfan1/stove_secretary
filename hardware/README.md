# Hardware planning

No hardware designs, wiring diagrams, parts selection, or calibration procedure are implemented here. Scope includes sensor selection, hood mounting, enclosure, power/wiring, cleaning and heat constraints, and device bring-up under [C1](../docs/components/C1-physical-sensing-module.md#c1). Sensor-alignment responsibilities belong to [C2](../docs/components/C2-capture-and-observation-management.md#c2); software acquisition is provisionally under [device](../apps/device/README.md).

The [pitch ownership proposal](../docs/sources/pitch.md#division-of-work) suggests Luke for sensing/electrical integration, Luke and Zayd for placement/cleaning/user safety, and shared integration/testing. These are proposed human ownership areas, not accepted assignments or contract owners.

Next useful work: inspect candidate mounting locations and evidence quality ([E01](../docs/experiments/README.md#e01)), then compare thermal value on target cookware ([E03](../docs/experiments/README.md#e03)). Canonical questions remain in [C1 gaps](../docs/components/C1-physical-sensing-module.md#design-gaps) and [C2 gaps](../docs/components/C2-capture-and-observation-management.md#design-gaps). Preserve the [prototype scope boundaries](../docs/requirements.md#r06).
