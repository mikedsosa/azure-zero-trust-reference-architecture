# ADR-0010: Conditional Access and PIM managed as code

- **Status:** Accepted
- **Date:** 2026-10-23
- **Deciders:** identity lead, security architect

## Context
Conditional Access is the main enforcement point (ADR-0001). Portal-only
changes are hard to review, easy to drift, and hard to roll back.

## Options considered
1. **Portal changes with a change ticket:** familiar, but no diff and no
   automated checks.
2. **Terraform (azuread/azurerm providers) in Git with PR review and CI
   checks.**
3. **Microsoft Graph scripts / PowerShell:** flexible, but no state or drift
   detection.

## Decision
Option 2. Policies start in report-only. Changes go through pull requests.
CI runs fmt/validate/tflint/checkov/gitleaks. A scheduled `plan` detects
drift. Break-glass accounts are never managed by Terraform.

## Consequences
- Terraform state is sensitive and needs a secured remote backend.
- The deployment identity is effectively Tier 0. Use workload identity
  federation, a protected environment, and manual approval.
- Emergency portal changes must be reconciled back into code.

## Related
MCSB IM-7, PV-2, DS-3 · Companion lab repo
