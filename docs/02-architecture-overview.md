# 02: Architecture Overview

## Logical layers

```mermaid
flowchart TB
    subgraph L1["Governance"]
        GOV["Security strategy · risk register · policies<br/>Azure Policy initiatives · Purview Compliance Manager"]
    end
    subgraph L2["Policy decision"]
        CA["Entra Conditional Access<br/>(identity, device, risk, location, app)"]
        CAE["Continuous access evaluation"]
    end
    subgraph L3["Pillars"]
        direction LR
        I["Identity"] --- E["Endpoints"] --- N["Network"] --- D["Data"] --- A["Apps"] --- INF["Infrastructure"]
    end
    subgraph L4["Visibility, automation, orchestration"]
        XDR["Defender XDR"] --- SEN["Microsoft Sentinel"] --- EXP["Security Exposure Management"]
    end
    L1 --> L2 --> L3 --> L4
    L4 -. feedback: risk signals, revocation .-> L2
```

## Enterprise access model (privileged access)

```mermaid
flowchart LR
    subgraph CP["Control plane (Tier 0)"]
        T0["Entra ID, PIM, CA policies,<br/>AD DS DCs, Azure root MG,<br/>backup vault admin"]
    end
    subgraph MP["Management plane"]
        T1["Azure subscriptions / workloads,<br/>Intune, M365 workload admin"]
    end
    subgraph DUP["Data / workload plane"]
        T2["Apps, data, user access"]
    end
    PAW["Privileged access workstations<br/>+ phishing-resistant MFA + PIM"] --> CP
    ADMIN["Specialized admin devices<br/>+ PIM"] --> MP
    USERS["Users on compliant devices"] --> DUP
    CP --> MP --> DUP
```

## Access flow for a user reaching a private app

```mermaid
sequenceDiagram
    autonumber
    participant U as User (Intune-managed laptop)
    participant GSA as Global Secure Access client
    participant ENTRA as Entra ID + Conditional Access
    participant MDE as Defender for Endpoint
    participant CON as Private Network connector
    participant APP as Private app (on-prem / Azure)
    participant SOC as Defender XDR / Sentinel

    U->>GSA: Open https://finance.internal
    GSA->>ENTRA: Token request for the Private Access app segment
    ENTRA->>MDE: Device risk / compliance (via Intune)
    ENTRA-->>ENTRA: Evaluate CA: user risk, device compliant,<br/>phishing-resistant MFA, app sensitivity
    alt Policy satisfied
        ENTRA-->>GSA: Access token
        GSA->>CON: Tunnel for this app only
        CON->>APP: Forward request
        APP-->>U: Response
    else Policy not satisfied
        ENTRA-->>U: Block or remediate (enroll device / step-up MFA)
    end
    ENTRA-->>SOC: Sign-in + CA result logs
    MDE-->>SOC: Endpoint telemetry
    SOC-->>ENTRA: On incident, revoke sessions / mark user compromised (CAE)
```

## Capability map

| Pillar | Primary capabilities | Licensing note (verify current plans) |
|---|---|---|
| Identity | Entra ID, Conditional Access, ID Protection, PIM, ID Governance, Defender for Identity | P1 baseline. P2 / Governance for risk, PIM, access reviews |
| Endpoints | Intune, Defender for Endpoint, Windows LAPS, security baselines | E3 + Defender for Endpoint P2 or E5 Security |
| Network | Entra Private Access / Internet Access, Azure Firewall Premium, DDoS Protection, Private Link, WAF | Global Secure Access licensing is separate |
| Data | Purview Information Protection, DLP, Insider Risk, Audit, Key Vault / Managed HSM | E5 Compliance for advanced features |
| Applications | Entra app registrations, managed identities, Defender for Cloud Apps, API Management, Front Door WAF, GitHub Advanced Security / Defender for Cloud DevOps | Mixed |
| Infrastructure | Azure landing zones, Azure Policy, Defender for Cloud (CSPM + workload plans), Azure Arc, Azure Backup | Consumption-based |
| SecOps | Defender XDR, Microsoft Sentinel, Security Exposure Management, Security Copilot (optional) | Sentinel is consumption-based |
