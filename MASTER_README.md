# Phoenix CAMEO Programme — Master README

> **Programme:** Phoenix CAMEO MBSE  
> **Document Type:** README  
> **Generated:** 2026-04-08  
> **Components Covered:** WAP · TWC · FlexNet · CST · CSM

---

## Contents

- [WAP — Web Application Platform (WAP)](#wap--web-application-platform-wap)
- [TWC — Teamwork Cloud (TWC)](#twc--teamwork-cloud-twc)
- [FLEXNET — FlexNet License Server](#flexnet--flexnet-license-server)
- [CST — Cameo Simulation Toolkit (CST)](#cst--cameo-simulation-toolkit-cst)
- [CSM — Cameo Systems Modeler (CSM)](#csm--cameo-systems-modeler-csm)

---

## WAP — Web Application Platform (WAP)

> **Source:** `wap/docs/README.md`

# Web Application Platform (WAP) Documentation Repository

**Product:** Web Application Platform (Service Tier)  
**Deployment Tier:** Stateless / Scalable  
**Author:** Iain Reid  
**Source PRD:** `PRD_2_Web_Application_Platform_VM.md`  
**Repository Status:** Scaffolded — documentation in progress

### Purpose

This repository holds all mandatory documentation artefacts for the Web Application Platform (WAP) VM deployment. WAP delivers web-based and service-oriented capabilities layered on Teamwork Cloud, including:

- Cameo Collaborator
- Resources Application
- Document Exporter
- Server-side CST simulation services

### Compliance Targets

| Standard | Scope |
|---|---|
| NIST SP 800-53 | Security controls |
| OWASP Top 10 | Web application threat coverage |
| DISA STIG | OS and application hardening |
| CIS Benchmark Level 2 | Host hardening |
| FIPS 140-3 | Cryptographic standards |

### Master Document Navigation (WAP)

| # | Document | File |
|---|---|---|
| 1 | Architecture Overview | `MASTER_01_architecture_overview.md` |
| 2 | Inside-Out Architecture | `MASTER_02_inside_out_architecture.md` |
| 3 | User Guide | `MASTER_03_user_guide.md` |
| 4 | Administrator Guide | `MASTER_04_admin_guide.md` |
| 5 | API & Integration Guide | `MASTER_05_api_integration_guide.md` |
| 6 | Security Analysis | `MASTER_06_security_analysis.md` |
| 7 | Compliance Mapping | `MASTER_07_compliance_mapping.md` |
| 8 | RBAC Definition | `MASTER_08_rbac_definition.md` |
| 9 | RACI Matrix | `MASTER_09_raci_matrix.md` |
| 10 | Maintenance Guide | `MASTER_10_maintenance_guide.md` |
| 11 | Supplementary (Simulation Services) | `MASTER_11_supplementary.md` |
| 12 | Deployment Guide | `MASTER_12_deployment_guide.md` |

---

## TWC — Teamwork Cloud (TWC)

> **Source:** `twc/docs/README.md`

# Teamwork Cloud — Core Repository VM

**Product:** Teamwork Cloud (Repository Tier)  
**Deployment Tier:** Critical / Stateful  
**Author:** Iain Reid  

### Purpose

Teamwork Cloud provides the authoritative collaboration, versioning, and persistence layer for Cameo-based MBSE tooling. This VM hosts Teamwork Cloud core services, Cassandra, and Zookeeper on a dedicated, hardened virtual machine within an air-gapped enterprise VMware environment.

### Compliance Standards

- NIST SP 800-53
- OWASP Top 10
- DISA STIG
- CIS Benchmark Level 2
- FIPS 140-3

### Security Note

**Never commit credentials, certificates, private keys, or secrets to this repository.** All secrets must be managed via the enterprise secrets management solution.

---

## FLEXNET — FlexNet License Server

> **Source:** `flexnet/docs/README.md`

# FlexNet License Server VM

**Product:** FlexNet Publisher (lmgrd / lmadmin)  
**OS:** RHEL 9 — STIG-hardened, air-gapped  
**Author:** Iain Reid  
**Classification:** OFFICIAL — SENSITIVE

### Purpose

This repository contains all build, deployment, operational, and compliance artefacts for the FlexNet license server VM that underpins Dassault Systèmes product authorisation within the Phoenix programme. The server is mission-critical: any outage causes complete tool unavailability.

### Served Products

| Product | Short Name |
|---------|-----------|
| Cameo Systems Modeler | CSM |
| Cameo Simulation Toolkit | CST |
| Teamwork Cloud | TWC |
| Web Application Platform | WAP |

### Non-Functional Requirements

| Requirement | Target |
|-------------|--------|
| Availability | ≥ 99.9% |
| Recovery | Snapshot-based, documented |
| Crypto | FIPS 140-3 |
| OS Hardening | DISA STIG RHEL 9 |
| Auth | AD-backed RBAC |
| Audit logging | Retained per AU-11 |

---

## CST — Cameo Simulation Toolkit (CST)

> **Source:** `cst/docs/README.md`

# Cameo Simulation Toolkit (CST)

**Product:** Cameo Simulation Toolkit  
**Deployment Tier:** Client (Windows) and Optional Server-Side Execution  
**Platform:** Windows Server 2025 (server-side); Windows 10/11 (client-side)  
**Author:** Iain Reid  

### Purpose

CST extends Cameo Systems Modeler (CSM) by enabling execution, validation, and analysis of SysML behavioural models — including state machines, activities, parametrics, and executable constraints. Simulation may run locally within the CSM client or remotely via the Web Application Platform (WAP) on Windows Server 2025.

### Compliance Standards

- NIST SP 800-53 Rev 5
- OWASP Top 10
- DISA STIG (Windows Server 2025)
- CIS Benchmark Level 2 (Windows Server 2025)
- FIPS 140-3

### Platform Notes

| Mode | Platform | Notes |
|------|----------|-------|
| Client-side execution | Windows 10 / 11 | CST plugin within CSM |
| Server-side execution | Windows Server 2025 | Optional; runs under WAP |

---

## CSM — Cameo Systems Modeler (CSM)

> **Source:** `csm/docs/README.md`

# Cameo Systems Modeler (CSM) Client

**Product:** Cameo Systems Modeler  
**Delivery:** Numecent Application Virtualisation on VMware Horizon Persistent VDI  
**Author:** Iain Reid  

### Documentation Index

| # | File | Title |
|---|---|---|
| 1 | `01_architecture_overview.md` | Client Architecture Overview (VDI + Numecent) |
| 2 | `02_logical_component_diagram.md` | Logical Component Diagram |
| 3 | `03_user_guide.md` | User Guide |
| 4 | `04_administrator_support_guide.md` | Administrator / Support Guide |
| 5 | `05_integration_guide.md` | Integration Guide (License Server, TWC) |
| 6 | `06_security_analysis_threat_model.md` | Security Analysis & Threat Model |
| 7 | `07_compliance_mapping.md` | Compliance Mapping (NIST, OWASP, STIG, CIS, FIPS) |
| 8 | `08_rbac_definition.md` | RBAC Definition |
| 9 | `09_raci_matrix.md` | RACI Matrix |
| 10 | `10_maintenance_patch_guide.md` | Maintenance & Patch Guide |
| 11 | `11_incident_support_runbook.md` | Incident & Support Runbook |
| 12 | `12_vdi_numecent_deployment_guide.md` | VDI & Numecent Deployment Guide |

---

## Programme Notes

This master README consolidates the five component repositories of the Phoenix CAMEO MBSE toolchain:

```
Phoenix/CAMEO/
├── wap/          Web Application Platform (WAP VM)
├── twc/          Teamwork Cloud (Core Repository VM)
├── flexnet/      FlexNet License Server VM
├── cst/          Cameo Simulation Toolkit (CST)
├── csm/          Cameo Systems Modeler (CSM Client / VDI)
└── master/       ← Master consolidated documents (this folder)
```

All master documents are generated by `merge_docs.py` located in the `Phoenix/CAMEO/` root. To regenerate after source updates:

```bash
cd C:\Users\ireid5\Documents\Phoenix\CAMEO
python merge_docs.py
```

---

*Generated: 2026-04-08 | Classification: OFFICIAL — SENSITIVE | Author: Iain Reid*
