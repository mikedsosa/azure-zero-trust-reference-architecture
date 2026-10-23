# ADR-0005: Azure landing zones with policy as guardrails

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** security architect, cloud platform lead

## Context
14 subscriptions created ad hoc. Inconsistent logging, public endpoints, and
no central policy (F6). The team is too small to review every deployment.

## Options considered
1. **Manual review gate for every deployment:** slow and doesn't scale.
2. **Azure landing zones (CAF) with Azure Policy at management groups:**
   preventive and detective controls applied automatically.
3. **Custom-built governance scripts:** flexible, but a maintenance burden.

## Decision
Option 2. Adopt the Azure landing zone MG hierarchy, deployed and maintained
as IaC. Assign the MCSB initiative (audit) org-wide, plus deny policies for
the highest-risk patterns (public IPs in Corp, storage public access, missing
diagnostic settings via DeployIfNotExists). Migrate existing subscriptions in
waves.

## Consequences
- Deny policies can break existing deployments. Run them in audit first and
  use exemptions with expiry dates.
- Platform team owns the policy-as-code repo and the exemption process.

## Related
F6 · MCSB PV-1, PV-2, AM-2, LT-3, GS-2 (v1) · Infrastructure pillar
