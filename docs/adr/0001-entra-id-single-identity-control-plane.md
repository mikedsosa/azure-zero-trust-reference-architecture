# ADR-0001: Entra ID as the single identity control plane

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** CISO, security architect, identity lead

## Context
Two AD DS forests, several SaaS apps with local accounts, and Azure workloads
using client secrets (F1, F2, F8). Policy can't be applied consistently when
identities live in several systems.

## Options considered
1. **Keep AD DS as primary, federate to cloud (AD FS):** familiar, but
   on-prem federation infrastructure becomes Tier 0 and a known attack target,
   and Conditional Access signals are weaker for federated flows.
2. **Third-party IdP in front of everything:** strong features, but a second
   policy engine to integrate with Microsoft 365 risk and device signals, plus
   added cost and skills for a 9-person team.
3. **Entra ID as the authority, AD DS synced for legacy:** one policy engine
   (Conditional Access) with native device, risk, and app signals.

## Decision
Option 3. Password hash sync + seamless SSO (no AD FS). All new apps integrate
with Entra. Admin accounts for cloud are **cloud-only**, so on-prem compromise
doesn't automatically extend to the cloud.

## Consequences
- Conditional Access becomes the most critical configuration (see ADR-0010).
- Sync servers are Tier 0 assets and must be hardened.
- Legacy Kerberos/NTLM apps need Private Access / App Proxy or modernization.

## Related
F1, F2, F8 · MCSB IM-1, IM-2, IM-5 · ADR-0010
