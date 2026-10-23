# ADR-0003: Just-in-time admin through PIM, no standing privilege

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** CISO, identity lead, GRC

## Context
About 40 permanent Global Administrators (F1). Standing privilege turns any
single compromised admin into a tenant compromise and makes reviews meaningless.

## Options considered
1. **Reduce GA count, keep the rest permanent:** smaller blast radius, but
   still standing access.
2. **PIM eligibility for all privileged Entra and Azure roles**, with
   approval for Tier 0 and time-bound activation.
3. **Third-party PAM vault for cloud roles:** strong for on-prem credentials,
   but duplicates native capability for Entra/Azure roles.

## Decision
Option 2 for Entra and Azure roles. Permanent assignment only for the two
break-glass accounts and up to three named GAs during transition. Tier 0
activations need approval and a ticket number. Max activation is 4 hours.
Quarterly access reviews remove unused eligibility.

## Consequences
- Requires Entra ID P2 (or ID Governance) for admins (decision D1).
- Admins have to activate before they can work, which slows them slightly.
  Runbooks need an "activate first" step.
- Option 3 stays on the table for on-prem Tier 0 credentials (Phase 4).

## Related
F1 · MCSB PA-1, PA-2, PA-4, PA-7 · Lab: `pim.tf`
