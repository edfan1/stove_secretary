# Mobile / interaction application

**Implementation:** README placeholder only; language/framework and web-versus-native approach remain open. This is the provisional home for [C7](../../docs/components/C7-app-and-interaction-layer.md#c7).

Read state through the backend boundary ([IC04](../../docs/contracts/register.md#ic04)); submit interaction/mutation intent via [IC06](../../docs/contracts/register.md#ic06), and receive presentation requests through [IC07](../../docs/contracts/register.md#ic07). The logical C4 read dependency does not require a direct network connection. See C7 for authoritative-state and pending/confirmed behavior.

Next proposed step: prototype setup, session controls, state display, and correction flows before selecting a stack ([E05](../../docs/experiments/README.md#e05)). [C7 design gaps](../../docs/components/C7-app-and-interaction-layer.md#design-gaps) include disconnected behavior and background delivery. A later simple client can exercise the replay slice. Dependencies, startup, packaging, and compatibility with older installed clients still need implementation.
