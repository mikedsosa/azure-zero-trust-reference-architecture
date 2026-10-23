# ADR-0004: Entra Private Access replaces client VPN

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** security architect, network lead, CISO

## Context
VPN gives network-level access to most server subnets (F3). Any compromised
VPN session can move laterally.

## Options considered
1. **Keep VPN, add internal segmentation:** less user change, but access
   remains network-centric and segmentation is costly on-prem.
2. **Third-party ZTNA/SSE:** mature options, but a separate policy engine
   from Conditional Access and another agent.
3. **Microsoft Entra Private Access (Global Secure Access):** per-app access
   evaluated by Conditional Access, using the same identity and device signals.

## Decision
Option 3. Start with Quick Access for discovery, then move to per-app
segments with CA per segment. Retire VPN group by group (decision D3).

## Consequences
- Requires the GSA client on managed devices and connectors near the apps.
- UDP-heavy or unusual protocols need testing. Some apps may keep a scoped
  VPN path as a time-boxed exception.
- Separate licensing (R2).

## Related
F3 · MCSB NS-1, NS-9, IM-7 · Network pillar
