# ADR-0006: Default sensitivity labels with Purview

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** GRC lead, data owners, legal, CISO

## Context
No classification and broad "anyone" links (F5). AI assistants will surface
whatever users can already reach.

## Options considered
1. **Manual labeling only:** low coverage in practice.
2. **Default label + auto-labeling for regulated patterns + mandatory labels
   on Confidential containers.**
3. **Full encryption by default:** strong, but breaks collaboration and
   search too early.

## Decision
Option 2. Four-tier taxonomy. Default "General". Auto-label regulated data
(simulation first). Encryption only on Highly Confidential. DLP in simulation
mode before enforcement.

## Consequences
- Needs legal/business sign-off on the taxonomy (dependency in roadmap).
- Some features need E5 Compliance (decision D1).
- Oversharing cleanup gates broad AI assistant rollout.

## Related
F5 · MCSB DP-1, DP-2 · Data pillar
