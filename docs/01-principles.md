# 01: Principles

## Zero Trust principles (Microsoft)

| Principle | What it means here |
|---|---|
| **Verify explicitly** | Every access decision uses identity, device, location, risk, and session signals, not network location |
| **Use least privilege access** | Just-in-time and just-enough access, risk-based adaptive policies, and data protection that travels with the data |
| **Assume breach** | Minimize blast radius, segment access, encrypt end to end, and use analytics to detect and respond |

## Design principles for this architecture

| # | Principle | Rationale | Consequence |
|---|---|---|---|
| P1 | **Identity is the primary control plane** | Users, devices, and workloads all authenticate to Entra ID, so one policy engine can see every request | Conditional Access becomes the most critical configuration in the estate and must be managed as code (ADR-0010) |
| P2 | **No standing privilege** | Most damaging intrusions abuse persistent admin rights | PIM for Entra and Azure roles, separate admin accounts, privileged access workstations for Tier 0 |
| P3 | **Device health is an access signal** | A valid credential on a compromised device is still a compromise | Intune compliance + Defender for Endpoint risk feed Conditional Access (ADR-0007) |
| P4 | **Connect users to apps, not networks** | Network-level VPN access gives broad lateral movement | Per-app access via Entra Private Access, then retire full-tunnel VPN (ADR-0004) |
| P5 | **Protect data where it lives and travels** | Data leaves the network boundary by design | Sensitivity labels, DLP, and encryption at the data layer (ADR-0006) |
| P6 | **Guardrails over gates** | A small team can't review every deployment | Azure Policy and landing zones enforce baselines automatically (ADR-0005) |
| P7 | **Telemetry by default, retained, centralized** | You can't respond to what you can't see | Every pillar ships logs to Defender XDR / Sentinel (ADR-0008) |
| P8 | **Report-only → pilot → enforce** | Avoid self-inflicted outages and build stakeholder trust | Every enforcement control in the roadmap has a staged rollout |
| P9 | **Prefer integrated, managed services** | Constraint: 9-person team | Fewer bespoke integrations. Accept some vendor concentration risk (tracked in roadmap risks) |
| P10 | **Recoverability is a security control** | Ransomware targets backups and the admin plane | Immutable backups with a separate admin plane (ADR-0009) |

## Zero Trust adoption framework alignment

Microsoft's Zero Trust adoption framework organizes work into business
scenarios. This roadmap uses them as program workstreams:

| Business scenario | Primary pillars | Roadmap phase where it lands |
|---|---|---|
| Rapidly modernize your security posture | Identity, infrastructure, SecOps | Phase 1-2 |
| Secure remote and hybrid work | Identity, endpoints, network | Phase 1-3 |
| Identify and protect sensitive business data | Data, applications | Phase 2-3 |
| Prevent or reduce business damage from a breach | All, especially privileged access and backup | Phase 1-3 |
| Meet regulatory and compliance requirements | Data, infrastructure, governance | Phase 2-4 |

The framework's lifecycle (define strategy → plan → ready → adopt → govern →
manage) is reflected in the roadmap's entry and exit criteria.
