# 03: Mapping to MCRA and MCSB

## How to read this mapping

- **MCSB** control IDs come from the *Microsoft cloud security benchmark v2
  (preview)*, which Microsoft lists as superseding v1. v2 has no Governance
  and Strategy domain, so **GS-x IDs come from MCSB v1** and are labeled that
  way. All IDs and titles were checked against Microsoft Learn on 2026-10-23.
  The domain is labeled in each row.
- A mapping means the design **addresses** the control's intent. It is not a
  compliance attestation. Microsoft's own guidance says the same of its
  framework mappings.
- **MCRA** references point to the diagrams/sections listed on the MCRA
  overview page (April 2025 version): capabilities, Zero Trust user access,
  security operations, multicloud and cross-platform, attack chain coverage,
  infrastructure and development, and security organizational functions.

## Pillar → MCRA

| This architecture | MCRA diagram / section |
|---|---|
| Identity pillar, Conditional Access framework, access flow sequence | Zero Trust user access. Microsoft cybersecurity capabilities (identity & access) |
| Privileged access / enterprise access model | Microsoft cybersecurity capabilities (privileged access). Security organizational functions |
| Endpoints pillar | Microsoft cybersecurity capabilities (endpoints & devices) |
| Network pillar (SSE, hub-spoke, WAF) | Zero Trust user access (Security Service Edge). Infrastructure and development |
| Data pillar | Microsoft cybersecurity capabilities (information protection) |
| Applications pillar (DevSecOps, APIs, AI apps) | Infrastructure and development |
| Infrastructure pillar (landing zones, Defender for Cloud, Arc) | Infrastructure and development. Multicloud and cross-platform capabilities |
| Security operations | Security operations (SecOps/SOC). Attack chain coverage |
| Roadmap and RACI | Security organizational functions. Guidance on prioritizing security work (threats section) |

## Design → MCSB controls

### Identity Management (IM), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| IM-1 | Centralize identity and authentication while ensuring isolation | ADR-0001. Identity pillar: single tenant, cloud-only admin accounts |
| IM-2 | Protect identity and authentication systems | Identity pillar: AD DS / sync server Tier 0, Defender for Identity, ID Protection |
| IM-3 | Manage application identities securely and automatically | Applications pillar: managed identities, workload identity federation |
| IM-4 | Authenticate server and services | Applications/network pillars: TLS, APIM token validation |
| IM-5 | Use single sign-on (SSO) for application access | Applications pillar: all apps via Entra |
| IM-6 | Use strong authentication controls | ADR-0002. CA001, CA003 |
| IM-7 | Restrict resource access based on conditions | Conditional Access framework (CA001-CA900), ADR-0007 |
| IM-8 | Restrict the exposure of credentials and secrets | Key Vault, secret scanning, no secrets in app settings |

### Privileged Access (PA), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| PA-1 | Separate and limit highly privileged/administrative users | Enterprise access model, ≤ 5 permanent GAs, separate admin accounts |
| PA-2 | Avoid standing access for user accounts and permissions | ADR-0003 PIM |
| PA-3 | Manage lifecycle of identities and entitlements | Identity governance: HR-driven JML, access packages |
| PA-4 | Review and reconcile user access regularly | Quarterly privileged access reviews, guest reviews |
| PA-5 | Set up emergency access | Break-glass accounts (identity pillar, companion lab runbook) |
| PA-6 | Use privileged access solution | PAWs for Tier 0, specialized admin devices |
| PA-7 | Follow just enough administration (least privilege) principle | Custom roles, scoped assignments, administrative units |
| PA-8 | Determine access process for cloud provider support | Customer Lockbox / support access approval (roadmap Phase 2) |

### Network Security (NS), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| NS-1 | Establish network segmentation boundaries | Hub-spoke, NSG/ASG, AVNM admin rules |
| NS-2 | Secure cloud native services with network controls | Private endpoints, public access disabled by policy |
| NS-3 | Deploy firewall at the edge of enterprise network | Azure Firewall Premium in hub |
| NS-4 | Deploy intrusion detection/intrusion prevention systems (IDS/IPS) | Azure Firewall Premium IDPS |
| NS-5 | Deploy DDoS protection | DDoS Protection on hub public IPs |
| NS-6 | Deploy web application firewall | Front Door / App Gateway WAF |
| NS-7 | Manage network security centrally and effectively | Firewall Manager / AVNM, policy-as-code |
| NS-8 | Detect and disable insecure services and protocols | Legacy auth block (CA002), NTLMv1 off, TLS minimums |
| NS-9 | Connect on-premises or cloud network privately | S2S VPN / ExpressRoute via hub, Entra Private Access |
| NS-10 | Ensure Domain Name System (DNS) security | Private DNS zones, DNS Private Resolver |

### Data Protection (DP), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| DP-1 | Discover, classify, and label sensitive data | ADR-0006, Purview |
| DP-2 | Monitor anomalies and threats targeting sensitive data | DLP, insider risk, Defender for Storage/Databases |
| DP-3 | Encrypt sensitive data in transit | TLS minimums, label encryption |
| DP-4 | Enable data at rest encryption by default | Platform encryption, TDE, BitLocker |
| DP-5 | Use customer-managed key option in data at rest encryption when required | CMK only where contract/regulation requires |
| DP-6 | Use a secure key management process | Key Vault / Managed HSM, rotation |
| DP-7 | Use a secure certificate management process | Key Vault certificates, expiry alerts |
| DP-8 | Ensure security of key and certificate repository | Key Vault RBAC, purge protection, private endpoint, Defender for Key Vault |

### Asset Management (AM), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| AM-1 | Track asset inventory and their risks | Defender for Cloud inventory, Exposure Management, Intune |
| AM-2 | Use only approved services | Azure Policy allowed services/regions |
| AM-3 | Ensure security of asset lifecycle management | Decommissioned MG, tagging, landing zone vending |
| AM-4 | Limit access to asset management | RBAC on management groups, PIM |
| AM-5 | Use only approved applications in virtual machine | Defender for Servers / app control on servers |

### Logging and Threat Detection (LT), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| LT-1 | Enable threat detection capabilities | Defender XDR, Defender for Cloud plans |
| LT-2 | Enable threat detection for identity and access management | ID Protection, Defender for Identity, identity analytics rules |
| LT-3 | Enable logging for security investigation | Diagnostic settings enforced by policy, Entra log export |
| LT-4 | Enable network logging for security investigation | VNet flow logs, firewall/WAF logs |
| LT-5 | Centralize security log management and analysis | Sentinel on central workspace |
| LT-6 | Configure log storage retention | Retention tiers per data type |
| LT-7 | Use approved time synchronization sources | Platform time sync. Domain controllers use an authoritative source |

### Incident Response (IR), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| IR-1 | Preparation - update incident response plan and handling process | Runbooks, quarterly tabletops |
| IR-2 | Preparation - setup incident notification | Action groups, Defender notification rules |
| IR-3 | Detection and analysis - create incidents based on high-quality alerts | XDR correlation, tuned analytics |
| IR-4 | Detection and analysis - investigate an incident | Unified SecOps investigation, hunting |
| IR-5 | Detection and analysis - prioritize incidents | Critical asset tagging, severity model |
| IR-6 | Containment, eradication and recovery - automate the incident handling | Playbooks, attack disruption |
| IR-7 | Post-incident activity - conduct lessons learned and retain evidence | Lessons-learned loop, evidence retention |

### Posture and Vulnerability Management (PV), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| PV-1 | Define and establish secure configurations | Landing zone policy baselines |
| PV-2 | Audit and enforce secure configurations | Azure Policy deny/audit, Defender CSPM |
| PV-3 | Define and establish secure configurations for compute resources | Security baselines, trusted launch |
| PV-4 | Audit and enforce secure configurations for compute resources | Azure Machine Configuration, Intune compliance |
| PV-5 | Perform vulnerability assessments | Defender Vulnerability Management, agentless scanning |
| PV-6 | Rapidly and automatically remediate vulnerabilities | Autopatch / update rings, Exposure Management prioritization |
| PV-7 | Conduct regular red team operations | Annual red team, purple-team detections (roadmap Phase 3+) |

### Endpoint Security (ES), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| ES-1 | Use Endpoint Detection and Response (EDR) | Defender for Endpoint / Defender for Servers |
| ES-2 | Use modern anti-malware software | Defender Antivirus with tamper protection |
| ES-3 | Ensure anti-malware software and signatures are updated | Intune / Defender update management |

### Backup and Recovery (BR), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| BR-1 | Ensure regular automated backups | Azure Backup policies by tier |
| BR-2 | Protect backup and recovery data | ADR-0009 immutable vault, MUA |
| BR-3 | Monitor backups | Backup alerts to SOC |
| BR-4 | Regularly test backup | Quarterly restore tests |

### DevOps Security (DS), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| DS-1 | Conduct threat modeling | Threat modeling at design (companion lab includes a STRIDE model) |
| DS-2 | Secure the software supply chain | Dependency scanning, pinned actions, signed artifacts |
| DS-3 | Secure the DevOps infrastructure | OIDC federation, least-privilege pipeline identities, branch protection |
| DS-4 | Integrate Static Application Security Testing (SAST) | CodeQL / SAST in CI |
| DS-5 | Integrate Dynamic Application Security Testing (DAST) | DAST against staging for internet-facing apps |
| DS-6 | Secure the workload lifecycle | Container image scanning, IaC scanning |
| DS-7 | Implement DevOps logging and monitoring | Defender for Cloud DevOps security, pipeline audit logs |

### Artificial Intelligence Security (AI), MCSB v2

| Control | Title | Where addressed |
|---|---|---|
| AI-1 | Ensure use of approved models | Applications pillar: approved model catalog |
| AI-2 | Implement multi-layered content filtering | Content filters / prompt shields |
| AI-3 | Adopt safety meta-prompts | App design standard for AI apps |
| AI-4 | Apply least privilege for agent functions | Agent identities with scoped permissions (CA400) |
| AI-5 | Ensure human-in-the-loop | Approval steps for high-impact agent actions |
| AI-6 | Establish monitoring and detection | Defender for Cloud AI threat protection, Purview DSPM for AI |
| AI-7 | Perform continuous AI Red Teaming | Roadmap Phase 4 |

### Governance and Strategy (GS), MCSB **v1**

| Control | Title | Where addressed |
|---|---|---|
| GS-1 | Align organization roles, responsibilities and accountabilities | Roadmap RACI |
| GS-2 | Define and implement enterprise segmentation/separation of duties strategy | Enterprise access model, MG hierarchy |
| GS-3 | Define and implement data protection strategy | Data pillar |
| GS-4 | Define and implement network security strategy | Network pillar |
| GS-5 | Define and implement security posture management strategy | Infrastructure pillar |
| GS-6 | Define and implement identity and privileged access strategy | Identity pillar |
| GS-7 | Define and implement logging, threat detection and incident response strategy | Security operations |
| GS-8 | Define and implement backup and recovery strategy | Infrastructure pillar (ransomware resilience) |
| GS-9 | Define and implement endpoint security strategy | Endpoints pillar |
| GS-10 | Define and implement DevOps security strategy | Applications pillar |
| GS-11 | Define and implement multi-cloud security strategy | Infrastructure pillar (Arc, Defender for Cloud connectors) |
