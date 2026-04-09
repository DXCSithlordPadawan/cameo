# Phoenix CAMEO — RACI Matrix

> **Programme:** Phoenix CAMEO MBSE
> **Document Type:** RACI Matrix
> **Classification:** OFFICIAL — SENSITIVE
> **Generated:** 2026-04-08
> **Author:** Iain Reid
> **Components Covered:** WAP · TWC · FlexNet · CST · CSM
---

**RACI Key:** R = Responsible | A = Accountable | C = Consulted | I = Informed

---

## Contents

1. [System Overview](#1-system-overview)
2. [WAP — Web Application Platform](#2-wap--web-application-platform)
   - [2.1 Stakeholders](#21-stakeholders)
   - [2.2 Accountability Map](#22-accountability-map)
   - [2.3 RACI Matrix](#23-raci-matrix)
   - [2.4 Incident Severity](#24-incident-severity)
3. [TWC — Teamwork Cloud](#3-twc--teamwork-cloud)
   - [3.1 Personas](#31-personas)
   - [3.2 Accountability Map](#32-accountability-map)
   - [3.3 RACI Table](#33-raci-table)
4. [FlexNet — License Server](#4-flexnet--license-server)
   - [4.1 Roles](#41-roles)
   - [4.2 Accountability Map](#42-accountability-map)
   - [4.3 RACI Table — Operations](#43-raci-table--operations)
   - [4.4 RACI Table — Licence Management](#44-raci-table--licence-management)
   - [4.5 RACI Table — Recovery](#45-raci-table--recovery)
5. [CST — Cameo Simulation Toolkit](#5-cst--cameo-simulation-toolkit)
   - [5.1 Persona Key](#51-persona-key)
   - [5.2 Accountability Map](#52-accountability-map)
   - [5.3 RACI Table — Simulation Operations](#53-raci-table--simulation-operations)
   - [5.4 RACI Table — Incident Response](#54-raci-table--incident-response)
   - [5.5 SLA Targets](#55-sla-targets)
6. [CSM — Cameo Systems Modeler](#6-csm--cameo-systems-modeler)
   - [6.1 Roles](#61-roles)
   - [6.2 Accountability Map](#62-accountability-map)
   - [6.3 RACI Table](#63-raci-table)

---

## 1. System Overview

The Phoenix CAMEO deployment is a five-component MBSE toolchain. The diagram below shows the primary accountable owner for each component and the key cross-component support flows.

```mermaid
graph TD
    subgraph LEGEND["RACI Key"]
        L1["R = Responsible"]
        L2["A = Accountable"]
        L3["C = Consulted"]
        L4["I = Informed"]
    end
    subgraph COMPONENTS["Phoenix CAMEO — Components & Accountable Owners"]
        WAP["WAP\nWeb Application Platform\nAccountable: Application Owner (AO)"]
        TWC["TWC\nTeamwork Cloud\nAccountable: Enterprise & Solution Architects (EA)"]
        FLX["FlexNet\nLicense Server\nAccountable: FlexNet Administrator (FNA)"]
        CST["CST\nCameo Simulation Toolkit\nAccountable: Platform Operations (PO)"]
        CSM["CSM\nCameo Systems Modeler\nAccountable: VDI Platform Engineer (VDI)"]
    end
    subgraph SHARED["Shared Support Roles"]
        SEC["Security Team / ISSO\nSecurity & Compliance"]
        PA["Platform / Programme Architect\nArchitecture Governance"]
        SD["Service Desk\nL1/L2 Support"]
    end
    WAP -- "LDAPS / AD Auth" --> TWC
    WAP -- "License Checkout" --> FLX
    CSM -- "Server-side simulation" --> CST
    CSM -- "TWC REST API" --> TWC
    CSM -- "License Checkout" --> FLX
    CST -- "License Checkout" --> FLX
    SEC -. "Security reviews & compliance" .-> WAP
    SEC -. "Security reviews & compliance" .-> TWC
    SEC -. "Audit & SIEM" .-> FLX
    PA -. "Architecture governance" .-> WAP
    PA -. "Architecture governance" .-> TWC
    SD -. "L1/L2 incident triage" .-> WAP
    SD -. "L1/L2 incident triage" .-> CSM
```

| Component | Primary Accountable Role | Key Responsible Role | Security Oversight |
|-----------|--------------------------|----------------------|--------------------|
| WAP | Application Owner (AO) | Platform Administrator (PA) | Security Team (SEC) |
| TWC | Enterprise & Solution Architects (EA) | MBSE Tool Administrator (TA) | Cybersecurity Operations (CO) |
| FlexNet | FlexNet Administrator (FNA) | FlexNet Administrator (FNA) | ISSO |
| CST | Platform Operations (PO) | MBSE Tool Administrator (TA) | Platform Operations (PO) |
| CSM | VDI Platform Engineer (VDI) | VDI Platform Engineer (VDI) | Cybersecurity Compliance Officer (CCO) |

---

## 2. WAP — Web Application Platform

> **Source:** `wap/docs/09_raci_matrix.md` | **Status:** Draft 0.2 | **Doc Ref:** WAP-DOC-09
### 2.1 Stakeholders

| Role | Description |
|---|---|
| Platform Administrator (PA) | Technical operator of WAP VM |
| Application Owner (AO) | Business / functional owner of WAP capability |
| Security Team (SEC) | Information security and compliance |
| Network / Infrastructure (NET) | Networking, VMware, firewall |
| Active Directory Admin (ADA) | AD group management and service accounts |
| DXC Service Desk (SD) | Level 1/2 support and request handling |
| End Users (EU) | Systems engineers and reviewers |
| Vendor Support (VS) | No Magic / Dassault Systèmes support |
| Enterprise PKI Team (PKI) | Internal CA certificate issuance |

---

### 2.2 Accountability Map

```mermaid
graph LR
    AO["Application Owner (AO)\n— Accountable across all activities —"]
    subgraph OPS["Operations"]
        PA["Platform Administrator (PA)\nResponsible: service, monitoring,\nlogs, patching, incidents"]
    end
    subgraph ACCESS["Access Management"]
        ADA["AD Admin (ADA)\nResponsible: user access,\naccount provisioning/removal"]
        SD["Service Desk (SD)\nResponsible: request processing,\nleaver removal"]
    end
    subgraph SECURITY["Security & Compliance"]
        SEC["Security Team (SEC)\nResponsible: STIG scans,\nthreat model, incident response"]
        PKI["Enterprise PKI (PKI)\nResponsible: TLS certificate\nissuance & renewal"]
    end
    subgraph INFRA["Infrastructure"]
        NET["Network / Infra (NET)\nResponsible: firewall rules,\nVM networking"]
        VS["Vendor Support (VS)\nResponsible: vendor escalation\nand P1 resolution support"]
    end
    AO -->|"Accountable"| OPS
    AO -->|"Accountable"| ACCESS
    AO -->|"Accountable"| SECURITY
    AO -->|"Accountable"| INFRA
    PA -->|"Escalates"| VS
    PA -->|"Coordinates"| SEC
    SD -->|"Escalates"| PA
```

---

### 2.3 RACI Matrix

| Activity | PA | AO | SEC | NET | ADA | SD | EU | VS | PKI |
|---|---|---|---|---|---|---|---|---|---|
| **Deployment & Infrastructure** |||||||||
| Initial WAP VM provisioning and OS deployment | R | A | C | C | I | I | I | I | I |
| OS hardening (CIS L2 / DISA STIG) | R | A | C | I | I | I | I | C | I |
| WAP software installation and configuration | R | A | C | I | I | I | I | C | I |
| TLS certificate request and issuance | C | A | C | I | I | I | I | I | R |
| AD service account creation | C | A | I | I | R | I | I | I | I |
| Firewall rules (WAP port allowlist) | C | A | C | R | I | I | I | I | I |
| **Operations** |||||||||
| WAP service start / stop / restart | R | A | I | I | I | I | I | I | I |
| Health monitoring and alerting | R | A | C | I | I | I | I | I | I |
| Log review and SIEM integration | R | A | R | I | I | I | I | I | I |
| **Access Management** |||||||||
| User access request processing | I | A | I | I | R | R | I | I | I |
| Role assignment approval | A | A | C | I | I | C | I | I | I |
| Quarterly access review | R | A | R | I | C | I | I | I | I |
| Leaver account removal | I | A | I | I | R | R | I | I | I |
| **Maintenance & Patching** |||||||||
| OS patch review and application | R | A | C | I | I | I | I | I | I |
| WAP application update application | R | A | C | I | I | I | I | C | I |
| TLS certificate renewal (60-day threshold) | R | A | C | I | I | I | I | I | C |
| **Security & Compliance** |||||||||
| STIG compliance scan execution | R | A | R | I | I | I | I | I | I |
| Threat model review | C | A | R | I | I | I | I | I | I |
| Security incident response | R | A | R | C | C | R | I | C | I |
| **Incident Management** |||||||||
| P1 incident declaration | A | A | C | C | I | R | I | I | I |
| P1 incident resolution coordination | R | A | C | C | I | R | I | C | I |
| Vendor support engagement | R | A | I | I | I | I | I | R | I |
| Post-incident review | R | A | R | C | I | C | I | C | I |

---

### 2.4 Incident Severity

```mermaid
graph TD
    P1["P1 — Critical\nWAP completely unavailable\nAll users impacted\nResponse: 30 minutes\nOwner: AO / PA"]
    P2["P2 — High\nMajor feature unavailable\nSignificant user impact\nResponse: 2 hours\nOwner: PA"]
    P3["P3 — Medium\nDegraded performance\nPartial feature loss\nResponse: 8 hours\nOwner: PA"]
    P4["P4 — Low\nMinor / cosmetic issue\nNon-blocking\nResponse: Next business day\nOwner: SD"]
    P1 -->|"Downgrade when stabilised"| P2
    P2 -->|"Downgrade when stabilised"| P3
    P3 -->|"Downgrade when stabilised"| P4
```

| Severity | Definition | Response Target |
|---|---|---|
| P1 — Critical | WAP completely unavailable; all users impacted | 30 minutes |
| P2 — High | Major feature unavailable; significant user impact | 2 hours |
| P3 — Medium | Degraded performance; partial feature loss | 8 hours |
| P4 — Low | Minor issue; cosmetic or non-blocking | Next business day |

---

## 3. TWC — Teamwork Cloud

> **Source:** `twc/docs/09_raci_matrix.md` | **Status:** Not Started 0.1-DRAFT | **Doc Ref:** DOC-09
### 3.1 Personas

| Code | Persona |
|------|---------|
| SE | Systems Engineers |
| TA | MBSE Tool Administrators |
| CO | Cybersecurity Operations |
| EA | Enterprise & Solution Architects |

---

### 3.2 Accountability Map

```mermaid
graph LR
    EA["Enterprise & Solution Architects (EA)\n— Accountable: Architecture, Deployment, Projects —"]
    TA["MBSE Tool Administrators (TA)\n— Accountable: User Roles, Health, Backups,\nPatching, Incident Response, RBAC Review —"]
    CO["Cybersecurity Operations (CO)\n— Accountable: OS Hardening, Security Reviews,\nAudit Logs, RBAC Review —"]
    subgraph ACTIVITIES["Activity Areas"]
        ARCH["Architecture & Deployment"]
        MODELLING["Modelling & Projects"]
        PLATFORM["Platform Operations"]
        SECURITY["Security & Compliance"]
    end
    EA -->|"Accountable"| ARCH
    EA -->|"Accountable"| MODELLING
    TA -->|"Accountable"| PLATFORM
    CO -->|"Accountable"| SECURITY
    SE["Systems Engineers (SE)\nResponsible: model authoring,\nproject creation & commits"]
    SE -->|"Consults"| TA
    SE -->|"Consults"| EA
```

---

### 3.3 RACI Table

| Activity | SE | TA | CO | EA |
|----------|----|----|----|-----|
| Define architecture requirements | C | C | C | A/R |
| Deploy TWC VM | I | R | C | A |
| Harden OS baseline (STIG/CIS) | I | R | A | I |
| Configure AD integration | I | R | C | I |
| Manage TWC user roles | I | A/R | C | I |
| Create / manage projects | R | C | I | R |
| Commit model changes | R | I | I | R |
| Monitor service health | I | A/R | C | I |
| Perform backups | I | A/R | I | I |
| Apply OS patches | I | R | A | I |
| Conduct security reviews | I | C | A/R | C |
| Review audit logs | I | C | A/R | I |
| Incident response | I | R | A | I |
| Quarterly RBAC access review | I | R | A | C |

---

## 4. FlexNet — License Server

> **Source:** `flexnet/docs/09_raci_matrix.md` | **Status:** ✅ Complete | **Version:** 0.2.0
### 4.1 Roles

| Role ID | Role | Representative |
|---------|------|----------------|
| PA | Phoenix Programme Architect | `<PA_NAME>` |
| ISSO | Information Systems Security Officer | `<ISSO_NAME>` |
| SA | System Administrator (OS / VM / Hypervisor) | `<SA_NAME>` |
| FNA | FlexNet Administrator | `<FNA_NAME>` |
| PM | Programme Manager | `<PM_NAME>` |
| USER | End User (CSM / CST / TWC / WAP) | All tool users |

---

### 4.2 Accountability Map

```mermaid
graph TD
    subgraph OPERATIONS["Operations & Monitoring"]
        FNA["FlexNet Administrator (FNA)\nAccountable: service availability,\nreloads, stuck checkouts, credential rotation,\nlicence expiry, borrow approvals,\noutage detection, recovery runbook"]
        SA["System Administrator (SA)\nResponsible: OS / VM / hypervisor\npatching, VM snapshot restore,\nfull rebuild"]
    end
    subgraph GOVERNANCE["Governance & Security"]
        ISSO["ISSO\nAccountable: audit logs, SIEM alerts,\nOS hardening, ISSO notification,\npost-recovery validation"]
        PA["Programme Architect (PA)\nAccountable: licence renewal initiation,\nborrow approval, seat adequacy,\nfull rebuild coordination"]
        PM["Programme Manager (PM)\nAccountable: seat count adequacy"]
    end
    subgraph CONSUMERS["Consumers"]
        USER["End Users\n(CSM / CST / TWC / WAP)\nProcess borrow requests,\nreport outages"]
    end
    FNA -->|"Escalates"| ISSO
    FNA -->|"Notifies"| PA
    SA -->|"Reports to"| FNA
    USER -->|"Requests"| FNA
    USER -->|"Reports outage to"| FNA
    PA -->|"Approves"| FNA
    PM -->|"Approves seat count"| PA
```

---

### 4.3 RACI Table — Operations

| Activity | PA | ISSO | SA | FNA | PM | USER |
|----------|:--:|:----:|:--:|:---:|:--:|:----:|
| Monitor service availability | I | I | C | A/R | I | I |
| Restart service on failure | I | I | C | A/R | I | I |
| Force reload licence file (lmreread) | I | I | I | A/R | I | I |
| Remove stuck checkout (lmremove) | I | I | I | A/R | I | C |
| Rotate lmadmin credentials | I | A | I | R | I | I |
| Review audit logs (weekly) | I | A/R | C | C | I | I |
| Respond to SIEM alerts | I | A | C | R | I | I |

---

### 4.4 RACI Table — Licence Management

| Activity | PA | ISSO | SA | FNA | PM | USER |
|----------|:--:|:----:|:--:|:---:|:--:|:----:|
| Monitor licence expiry dates | I | I | I | A/R | I | I |
| Initiate licence renewal request | A | I | I | R | C | I |
| Approve offline licence borrow | A | C | I | R | I | C |
| Process borrow request (lmborrow) | I | I | I | A/R | I | R |
| Manage seat count adequacy | A | I | I | C | R | I |

---

### 4.5 RACI Table — Recovery

```mermaid
graph TD
    DETECT["Detect & Report Outage\nAccountable: FNA\nResponsible: FNA, SA (C), USER (R)"]
    RUNBOOK["Invoke Recovery Runbook\nAccountable: FNA\nConsulted: ISSO, SA"]
    SNAPSHOT["VM Snapshot Restore\nAccountable: SA\nConsulted: ISSO, FNA"]
    REBUILD["Full Rebuild from Scratch\nAccountable: PA\nResponsible: SA, FNA"]
    VALIDATE["Post-Recovery Validation\nAccountable: ISSO\nResponsible: FNA\nConsulted: PA, SA"]
    NOTIFY["ISSO Notification\nAccountable/Responsible: ISSO\nConsulted: PA"]
    DETECT -->|"Minor outage"| RUNBOOK
    RUNBOOK -->|"VM restore available"| SNAPSHOT
    RUNBOOK -->|"Rebuild required"| REBUILD
    SNAPSHOT -->|"Restored"| VALIDATE
    REBUILD -->|"Complete"| VALIDATE
    VALIDATE -->|"Security-relevant event"| NOTIFY
```

| Activity | PA | ISSO | SA | FNA | PM | USER |
|----------|:--:|:----:|:--:|:---:|:--:|:----:|
| Detect and report outage | I | I | C | A/R | I | R |
| Invoke recovery runbook (doc 05) | I | C | C | A/R | I | I |
| VM snapshot restore | I | C | A/R | C | I | I |
| Full rebuild from scratch | A | C | R | R | I | I |
| Post-recovery validation | C | A | C | R | I | I |
| ISSO notification (security relevant) | C | A/R | I | I | I | I |

---

## 5. CST — Cameo Simulation Toolkit

> **Source:** `cst/docs/09_raci_matrix.md` | **Status:** In Progress 0.2-DRAFT | **Doc Ref:** DOC-09
### 5.1 Persona Key

| Code | Persona | AD Role |
|------|---------|---------|
| SE | Systems Engineers | `CST_USER` |
| SS | Simulation Specialists | `CST_SIMULATION_SPECIALIST` |
| TA | MBSE Tool Administrators | `CST_ADMIN` |
| PO | Platform Operations | `CST_PLATFORM_OPS` |

---

### 5.2 Accountability Map

```mermaid
graph LR
    subgraph SIMULATION["Simulation Operations"]
        SE["Systems Engineers (SE)\nAccountable: local simulations,\ncancel own, view/export results"]
        SS["Simulation Specialists (SS)\nAccountable: server-side simulations,\ncancel own, view/export results\nResponsible: triage failures"]
        TA["MBSE Tool Administrators (TA)\nAccountable: all result exports, cancel any\n(incident), resolve plugin issues,\nescalate to vendor"]
        PO["Platform Operations (PO)\nAccountable: cancel any (incident),\nresolve service outages,\nescalate to vendor, post-incident review"]
    end
    subgraph INCIDENTS["Incident Escalation Path"]
        L1["L1 — SE / SS detect failure"]
        L2["L2 — SS triages failure"]
        L3["L3 — TA resolves plugin issue\nor PO resolves service outage"]
        L4["L4 — TA escalates to vendor\n(PO accountable)"]
        PIR["Post-Incident Review\nOwner: PO (A) / TA (R)"]
    end
    L1 -->|"Escalate"| L2
    L2 -->|"Plugin issue"| L3
    L2 -->|"Service outage"| L3
    L3 -->|"Cannot resolve"| L4
    L4 --> PIR
```

---

### 5.3 RACI Table — Simulation Operations

| Activity | SE | SS | TA | PO |
|----------|----|----|----|-----|
| Run local simulation | R/A | C | I | I |
| Run server-side simulation | R | A | C | I |
| Cancel simulation (own) | R/A | R/A | I | I |
| Cancel simulation (any — incident) | I | I | R | A |
| View simulation results | R | R | A | I |
| Export simulation results | R | R | A | I |

---

### 5.4 RACI Table — Incident Response

| Activity | SE | SS | TA | PO |
|----------|----|----|----|-----|
| Detect simulation failure (own) | R | R | I | I |
| Triage simulation failure | C | R | C | I |
| Resolve plugin issue | I | C | R/A | I |
| Resolve server-side service outage | I | I | C | R/A |
| Escalate to vendor support | I | I | R | A |
| Post-incident review | C | C | R | A |

---

### 5.5 SLA Targets

```mermaid
graph TD
    P1C["P1 — Critical\nCST server completely unavailable\nResponse: 15 min | Resolution: 4 hrs\nOwner: Platform Operations"]
    P2H["P2 — High\nAll simulations failing reproducibly\nResponse: 30 min | Resolution: 8 hrs\nOwner: MBSE Tool Administrator"]
    P3M["P3 — Medium\nDegraded / intermittent failure\nResponse: 2 hrs | Resolution: 2 business days\nOwner: MBSE Tool Administrator"]
    P4L["P4 — Low\nSingle-user cosmetic issue\nResponse: Next business day | Resolution: 5 business days\nOwner: MBSE Tool Administrator"]
    P1C -->|"Downgrade when stabilised"| P2H
    P2H -->|"Downgrade when stabilised"| P3M
    P3M -->|"Downgrade when stabilised"| P4L
```

| Severity | Description | Response Time | Resolution Target | Owner |
|----------|-------------|--------------|------------------|-------|
| P1 — Critical | CST server service completely unavailable | 15 minutes | 4 hours | Platform Operations |
| P2 — High | All simulations failing reproducibly | 30 minutes | 8 hours | MBSE Tool Administrator |
| P3 — Medium | Degraded performance; intermittent failure | 2 hours | 2 business days | MBSE Tool Administrator |
| P4 — Low | Single-user cosmetic issue | Next business day | 5 business days | MBSE Tool Administrator |

---

## 6. CSM — Cameo Systems Modeler

> **Source:** `csm/docs/09_raci_matrix.md` | **Status:** ✅ Done
### 6.1 Roles

| Code | Role |
|---|---|
| SE | Systems Engineer |
| LA | Lead Architect |
| MBSE | MBSE Tool Administrator |
| VDI | VDI Platform Engineer |
| CCO | Cybersecurity Compliance Officer |
| INFRA | Infrastructure (Licence Server / Network) |

---

### 6.2 Accountability Map

```mermaid
graph TD
    subgraph DEPLOYMENT["Deployment & Packaging"]
        VDI_D["VDI Platform Engineer (VDI)\nAccountable: gold image build,\nAV exclusions, VDI pool entitlement,\nOS patches"]
        MBSE_D["MBSE Tool Administrator (MBSE)\nAccountable: CSM Numecent package,\nfloating licence assignment,\nCSM version updates"]
        INFRA_D["Infrastructure (INFRA)\nAccountable: licence server connection,\nnetwork allow-list,\nAD account provisioning"]
    end
    subgraph OPERATIONS["Operations"]
        LA_O["Lead Architect (LA)\nAccountable: model approval\nand baselining"]
        SE_O["Systems Engineer (SE)\nAccountable: model authoring,\nincident reporting"]
        CCO_O["Cybersecurity Compliance Officer (CCO)\nAccountable: AV exclusions approval,\nsecurity compliance review,\nincident closure & review"]
    end
    subgraph INCIDENTS["Incident Management"]
        TRIAGE["Triage & Diagnose\nOwner: MBSE (R) / VDI (R)"]
        ESCALATE["Escalate to Vendor\nAccountable: MBSE"]
        CLOSE["Closure & Review\nAccountable: CCO"]
    end
    SE_O -->|"Reports incident"| TRIAGE
    LA_O -->|"Reports incident"| TRIAGE
    TRIAGE -->|"Vendor issue"| ESCALATE
    ESCALATE --> CLOSE
    MBSE_D -->|"Coordinates"| VDI_D
    INFRA_D -->|"Supports"| MBSE_D
```

---

### 6.3 RACI Table

| Activity | SE | LA | MBSE | VDI | CCO | INFRA |
|---|---|---|---|---|---|---|
| **Deployment & Packaging** |||||||
| Build VDI gold image | I | I | C | R/A | C | I |
| Package CSM in Numecent | I | I | R/A | I | C | I |
| Configure licence server connection | I | I | C | I | I | R/A |
| Configure network allow-list | I | I | C | C | C | R/A |
| Configure AV exclusions | I | I | C | R | A | I |
| **User Onboarding** |||||||
| Provision AD account | I | I | C | I | I | R/A |
| Assign VDI pool entitlement | I | I | C | R/A | I | I |
| Assign floating licence | I | I | R/A | I | I | C |
| **Operations** |||||||
| Author SysML models | R | R | I | I | I | I |
| Approve / baseline models | C | R/A | I | I | I | I |
| Monitor licence utilisation | I | I | R | I | A | C |
| Monitor VDI health | I | I | I | R/A | I | I |
| Apply OS patches | I | I | C | R/A | C | I |
| Update CSM package version | I | I | R/A | C | C | I |
| Review security compliance | I | I | C | C | R/A | I |
| **Incident Management** |||||||
| Report incident | R | R | I | I | I | I |
| Triage and diagnose | I | I | R | R | C | C |
| Escalate to vendor | I | I | R/A | C | I | I |
| Incident closure and review | I | I | C | C | R/A | I |

---

*Generated: 2026-04-08 | Classification: OFFICIAL — SENSITIVE | Author: Iain Reid*
