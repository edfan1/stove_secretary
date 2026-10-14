# Backend application

**Implementation:** README placeholder only; stack, storage, model provider, deployment host, and transport are open. Provisional home for [C4](../../docs/components/C4-session-state-and-history.md#c4) and [C5](../../docs/components/C5-assistant-harness.md#c5). [C3](../../docs/components/C3-observation-selection-and-optional-perception.md#c3) may span backend/device after experiments; [C6](../../docs/components/C6-observability-and-replay.md#c6) instrumentation spans applications.

Likely internal modules, proposed rather than code: API/interaction routing; evidence access; observation selection; sessions/state; harness/context assembly; validated tools; scheduling; intervention policy; provider adapters; telemetry. These are modules inside an application, not separate services. The harness is one part of the backend, not its synonym. Semantic boundaries live in the [contract register](../../docs/contracts/register.md).

Next proposed step: a replay-first observation → state → harness → client slice with correction, duplicate, and timer cases ([E04](../../docs/experiments/README.md#e04)). Select slice-specific implementation interfaces in a follow-up; do not infer final schemas from these placeholders. Deployment/offline behavior remains [O01](../../docs/decisions/README.md#o01). Dependencies, startup, and build/deployment commands do not exist yet.
