# 05: Identity Pillar Implementation

The identity pillar has a working-code companion:

**[mikedsosa/azure-entra-identity-hardening](https://github.com/mikedsosa/azure-entra-identity-hardening)**
(Terraform, azuread + azurerm providers)

> Status: the Terraform passes `terraform fmt`, `terraform validate`,
> `tflint`, and `checkov` locally. It has **not** been applied to a tenant
> yet. Deployment and validation are steps for the reader to run in a lab
> tenant.

## What maps to what

| This architecture | Companion lab |
|---|---|
| CA001-CA005 baseline policies | `conditional_access.tf`, report-only by default |
| CA006/CA007 risk-based policies | `conditional_access.tf`, behind `enable_risk_based_policies` (P2) |
| ADR-0002 phishing-resistant MFA for admins | CA003 with the built-in *Phishing-resistant MFA* authentication strength |
| ADR-0003 PIM, no standing admin | `pim.tf`: eligible Entra role + eligible Azure role that expires |
| PA-5 emergency access | Break-glass exclusion group (`groups.tf`) + runbook + sign-in alert |
| PA-7 least privilege | Custom "Lab VM Operator" role scoped to one resource group (`rbac.tf`) |
| LT-3 / LT-5 identity logging | Entra diagnostic setting → Log Analytics (`logging.tf`) |
| ADR-0010 CA as code | Whole repo + GitHub Actions (fmt, validate, tflint, checkov, gitleaks) |
| Identity threat analysis | `docs/threat-model.md` (STRIDE) |
| Incident response | `docs/runbook.md` (risky sign-in, compromised admin, break-glass) |

## Gaps between design and lab (next increments)

- CA100/CA101 device-based policies (need Intune).
- CA200 guest, CA300 workload identity, CA400 agent policies.
- Authentication methods policy (disable SMS/voice) as code.
- Access reviews and entitlement management.
- Alerts on CA policy and exclusion-group changes.
