# Stove Secretary

A cooking assistant mounted near an existing range hood: camera evidence, optional thermal sensing, explicit session history, and model-centered assistance through an app and potentially voice. The initial scope is one active vessel and bounded workflows; see the [overview](docs/overview.md).

**Project status:** Repository/documentation bootstrap complete; applications are unimplemented. The architecture is a working design and contracts are proposals, not tested guarantees. Backend and harness planning has received more attention than sensing, device integration, and app experience. That difference does not establish relative importance, complexity, or readiness. The seven components and current contracts are an initial organizing structure, not an exhaustive specification.

## Start here

Read the [overview](docs/overview.md), then use the [documentation index](docs/README.md) to find relevant component, contract, requirement, decision, or experiment IDs. The linked canonical documents form the knowledge graph; no separate graph service is required. Use the [maintenance workflow](docs/maintenance.md) when changing behavior or design.

## Repository map

| Location | Purpose |
| --- | --- |
| [apps/mobile](apps/mobile/README.md) | Presentation and interaction placeholder; stack open |
| [apps/backend](apps/backend/README.md) | State, harness, and backend modules placeholder |
| [apps/device](apps/device/README.md) | Local acquisition and buffering placeholder |
| [contracts](contracts/README.md) | Future concrete cross-application schemas |
| [hardware](hardware/README.md) | Physical integration and calibration planning |
| [evaluation](evaluation/README.md) | Proposed isolated replay and evaluation workflow |
| [scripts](scripts/README.md) | Future shared commands; none implemented |
| [docs](docs/README.md) | Maintained planning definitions and frozen source provenance |

The monorepo decision is recorded in [D001](docs/decisions/D001-monorepo.md#d001). Folder allocation is provisional: local capture/buffering in device, state/harness in backend, presentation in mobile; observation selection may span device/backend and observability is cross-cutting. Folder names do not settle physical deployment or offline behavior. Each application will eventually have its own dependencies, startup command, and build/deployment path. Shared source does not require synchronized releases; compatibility must accommodate older installed clients.

## Next work

Proposed first implementation task: a replay-first vertical slice taking recorded normalized observations and user input through session state and validated harness actions to a simple client. Include a correction, duplicate trigger, and timer expiry with isolated output; [E04](docs/experiments/README.md#e04) investigates relevant semantics. Choose the stack and slice-specific schemas in that follow-up task.

In parallel with software planning, evaluate placement and evidence quality ([E01](docs/experiments/README.md#e01), [E02](docs/experiments/README.md#e02)) and prototype setup, state display, and correction workflows ([E05](docs/experiments/README.md#e05)). Replay alone does not resolve sensing or app design. Deployment, model/provider, recipe representation, cadence, speech, retention, and performance targets remain open in the [decision register](docs/decisions/README.md).

See [CONTRIBUTING](CONTRIBUTING.md) for collaboration and [AGENTS](AGENTS.md) for agent guidance. No runnable applications or bootstrap-specific build commands are available yet.
