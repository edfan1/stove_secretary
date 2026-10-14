# Monorepo decision

## D001

**Status:** Accepted. **Date:** October 14, 2026. **Decision owner:** Not assigned. **Implementation status:** Repository/documentation scaffold created; applications unimplemented.

**Decision:** Maintain Stove Secretary in one existing Git repository.

**Authorization:** The user selected “Full bootstrap,” explicitly confirming one monorepo, then requested implementation of the resulting plan. This records the user's repository decision; it does not assert approval by every team member.

**Expected benefits:** Shared navigation across hardware/software/evaluation, atomic updates to code and affected contracts/docs, and easier integration review. These are expectations, not measured outcomes.

**Tradeoffs:** Requires care with independent application dependencies, compatibility, releases, and large-data boundaries; one repository does not remove coordination work.

**Consequences:** Use the [repository map](../../README.md#repository-map) as the initial scaffold. Directory allocation is a recommendation and remains provisional. This decision does not select a language, provider, deployment host, or synchronized release policy. Each application will have its own build/deployment path; older installed clients still require compatibility.

**Affected references:** [Overview](../overview.md), application READMEs in the repository map, and [contribution workflow](../../CONTRIBUTING.md). **Evidence:** Inspect the scaffold and its links; no experiment is required to claim performance benefits, and no performance claim is made. No superseded repository decision was supplied.
