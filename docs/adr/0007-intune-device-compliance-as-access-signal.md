# ADR-0007: Device compliance as a Conditional Access signal

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** endpoint lead, identity lead, security architect

## Context
Valid credentials on unmanaged or compromised devices can reach M365 data (F4).

## Options considered
1. **Block all unmanaged devices:** strong, but breaks contractor and BYOD use.
2. **Require compliant device for full access, browser-only for unmanaged.**
3. **Monitor only:** no protection.

## Decision
Option 2. Intune compliance includes Defender for Endpoint machine risk. CA100
requires a compliant device for M365 and Private Access apps. CA101 allows
browser-only sessions with no download on unmanaged devices. Mobile uses app
protection policies.

## Consequences
- Enrollment campaign and service desk load (R6).
- Compliance results must be reliable before enforcement (report-only first).

## Related
F4 · MCSB IM-7, ES-1, PV-4 · Endpoints pillar
