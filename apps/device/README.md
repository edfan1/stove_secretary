# Device application

**Implementation:** README placeholder only; language, device runtime, and deployment choices are open. Provisionally hosts acquisition software associated with [C1](../../docs/components/C1-physical-sensing-module.md#c1) and local capture/buffering in [C2](../../docs/components/C2-capture-and-observation-management.md#c2). Hardware responsibilities remain distinct even when one machine hosts multiple logical components.

The device boundary covers [IC01](../../docs/contracts/register.md#ic01) samples/configuration, [IC02](../../docs/contracts/register.md#ic02) normalized evidence access, and [IC09](../../docs/contracts/register.md#ic09) instrumentation. Optional C3 processing needs experimental justification; buffering does not settle backend hosting or offline timer behavior ([O01](../../docs/decisions/README.md#o01)).

Next proposed step: test camera placement, timestamps, gaps, and retrieval with target cooking evidence ([E01](../../docs/experiments/README.md#e01), [E02](../../docs/experiments/README.md#e02)). Read [C1 gaps](../../docs/components/C1-physical-sensing-module.md#design-gaps) and [C2 gaps](../../docs/components/C2-capture-and-observation-management.md#design-gaps) for calibration, connectivity, and provisioning questions. No capture daemon, startup command, or build path exists yet.
