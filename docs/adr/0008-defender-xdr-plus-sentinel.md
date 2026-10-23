# ADR-0008: Defender XDR + Microsoft Sentinel for security operations

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** SOC manager, CISO, security architect

## Context
No central SIEM. A small SOC. Telemetry spread across many consoles (F6).

## Options considered
1. **Third-party SIEM + Microsoft XDR:** keeps an existing skill set, but
   duplicates ingestion costs and loses some native correlation.
2. **Defender XDR + Microsoft Sentinel in the unified Defender portal:**
   native correlation, attack disruption, one console.
3. **MDR provider only:** fast coverage, but less internal visibility and
   control.

## Decision
Option 2, with an MDR partner for after-hours if leadership chooses (D2).
Detections are kept in source control and mapped to MITRE ATT&CK.

## Consequences
- Vendor concentration risk (R8), accepted and reviewed annually.
- Ingestion cost has to be managed (retention tiers, filtering at the DCR).

## Related
F6 · MCSB LT-1…LT-6, IR-1…IR-7 · Security operations
