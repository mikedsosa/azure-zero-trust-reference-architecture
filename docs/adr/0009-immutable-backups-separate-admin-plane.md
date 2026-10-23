# ADR-0009: Immutable backups with a separate admin plane

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** infrastructure lead, CISO

## Context
Backup admins share identities with server admins (F7). Ransomware operators
target backups before encrypting.

## Options considered
1. **Same tooling, stronger passwords:** doesn't change the blast radius.
2. **Azure Backup immutable vaults + soft delete + multi-user authorization
   (Resource Guard owned by a different team/subscription).**
3. **Offline/air-gapped copies only:** strong, but slow to restore.

## Decision
Option 2 for Azure and Arc workloads, plus a periodic offline copy for Tier 0.
Quarterly restore tests with evidence.

## Consequences
- Destructive backup operations need two people.
- Immutability is irreversible once locked, so validate retention first.

## Related
F7 · MCSB BR-1…BR-4 · Infrastructure pillar
