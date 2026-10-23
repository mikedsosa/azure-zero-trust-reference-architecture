# 00: Context, Scope, and Assumptions

> Everything about "Fabrikam" is **fictional** and was invented to give the
> architecture realistic constraints. Numbers describe the scenario. They
> are not measurements.

## Organization profile

| Attribute | Scenario value |
|---|---|
| Size | ~2,500 employees, ~300 contractors, ~150 partner users |
| Footprint | HQ plus 6 regional offices, ~60% hybrid/remote workers |
| Business | Professional services handling confidential client data (contracts, financials, some regulated personal data) |
| Productivity | Microsoft 365 E3 today, with budget approved to evaluate E5 security components |
| Identity | On-prem AD DS (2 forests after an acquisition) synced to one Entra tenant |
| Cloud | 14 Azure subscriptions created ad hoc. Some IaaS lift-and-shift, some App Service/AKS |
| Network | Hub datacenter, MPLS + site-to-site VPN, client VPN for remote users |
| Security team | 9 people: CISO, 2 architects/engineers, 4 SOC analysts (business hours), 2 GRC |
| Obligations | Client contractual security requirements, privacy law for personal data, annual external audit |

## Current-state findings (scenario)

| # | Finding | Zero Trust principle violated |
|---|---|---|
| F1 | ~40 permanent Global Administrators. Admins use their daily account | Least privilege |
| F2 | MFA registered for ~70% of users. Legacy auth allowed | Verify explicitly |
| F3 | Any VPN user can reach most server subnets | Assume breach |
| F4 | No device compliance check on access to M365 | Verify explicitly |
| F5 | No data classification. External sharing is "anyone with the link" | Least privilege |
| F6 | No landing zone. Diagnostic logs are inconsistent. No central SIEM | Assume breach |
| F7 | Backup admins use the same domain accounts as server admins | Assume breach |
| F8 | Workloads use client secrets stored in app settings | Verify explicitly |

## Scope

**In scope:** Entra ID tenant, Microsoft 365 workloads, Azure subscriptions,
corporate endpoints (Windows, macOS, iOS, Android), remote access, security
operations.

**Out of scope:** OT/IoT (no plant environment in the scenario), detailed
application code review, physical security, and HR processes beyond joiner/mover/leaver.

## Constraints

- **Budget:** licensing upgrades must be phased. Not every user gets E5 on day one.
- **People:** a 9-person security team, so designs must favor managed,
  integrated services over self-built tooling.
- **Change tolerance:** no big-bang cutovers. Every enforcement change gets a
  report-only or pilot stage.
- **Legacy:** ~25 line-of-business apps use Kerberos/NTLM or header auth.

## Assumptions

- A1: The organization keeps a single Entra tenant (no multi-tenant org).
- A2: Leadership will sponsor an 18-month program with quarterly reviews.
- A3: The service desk can absorb a temporary rise in MFA/device enrollment tickets.
- A4: Microsoft-native tooling is preferred where it meets requirements (see ADR-0008 for the trade-offs).
