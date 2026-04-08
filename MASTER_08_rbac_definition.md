# Phoenix CAMEO — Master RBAC Definition & Access Matrix

> **Programme:** Phoenix CAMEO MBSE  
> **Document Type:** RBAC Definition  
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

> **Source:** `wap/docs/08_rbac_definition.md` | **Status:** Draft 0.2 | **Doc Ref:** WAP-DOC-08

---

### Role Definitions (WAP)

| Role ID | Role Name | Description | AD Group (TBC) | Privilege Level |
|---|---|---|---|---|
| WAP-R01 | Platform Administrator | Full administrative access to WAP configuration, services, and audit logs | `SG-WAP-Admins` | Highest |
| WAP-R02 | Collaborator User | Read/write access to Cameo Collaborator — view, comment, annotate models | `SG-WAP-CollabUsers` | Standard |
| WAP-R03 | Collaborator Viewer | Read-only access to Cameo Collaborator | `SG-WAP-CollabViewers` | Read-only |
| WAP-R04 | Document Export User | Collaborator Viewer access + submit document export jobs | `SG-WAP-DocExport` | Functional |
| WAP-R05 | Simulation User | Collaborator Viewer access + submit and monitor CST simulation jobs | `SG-WAP-SimUsers` | Functional |
| WAP-R06 | API Consumer | Programmatic service account access to WAP REST API | `SG-WAP-API` | Service |

---

### Access Control Matrix (WAP)

✅ = Permitted | ❌ = Denied | 🔍 = Permitted with audit log entry

| Capability | R01 Admin | R02 Collab User | R03 Viewer | R04 Doc Export | R05 Sim User | R06 API |
|---|---|---|---|---|---|---|
| Login to web interface | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| API token authentication | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| List accessible projects | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Browse model elements and diagrams | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Add comments / annotations | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Submit document export job | ✅ 🔍 | ✅ | ❌ | ✅ 🔍 | ❌ | ✅ 🔍 |
| Download own export result | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ |
| View all users' export jobs | ✅ 🔍 | ❌ | ❌ | ❌ | ❌ | ❌ |
| Submit simulation job | ✅ 🔍 | ❌ | ❌ | ❌ | ✅ 🔍 | ✅ 🔍 |
| View own simulation job status | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Cancel any simulation job | ✅ 🔍 | ❌ | ❌ | ❌ | ❌ | ❌ |
| Access admin console | ✅ 🔍 | ❌ | ❌ | ❌ | ❌ | ❌ |
| Manage service configuration | ✅ 🔍 | ❌ | ❌ | ❌ | ❌ | ❌ |
| View audit logs | ✅ 🔍 | ❌ | ❌ | ❌ | ❌ | ❌ |

---

### Service Account Definitions (WAP)

| Account ID | Account Name | Purpose | Permissions | Rotation |
|---|---|---|---|---|
| WAP-SA-01 | `svc-wap-twc` | WAP → TWC REST API integration | Read access to all TWC projects | Quarterly |
| WAP-SA-02 | `svc-wap-bind` | LDAP bind account for AD authentication | Read-only AD bind — no write | Quarterly |

---

## TWC — Teamwork Cloud (TWC)

> **Source:** `twc/docs/08_rbac_definition_access_matrix.md` | **Status:** Not Started 0.1-DRAFT | **Doc Ref:** DOC-08

---

### Role Definitions (TWC)

| Role | Description | AD Group (Placeholder) |
|------|-------------|----------------------|
| TWC-Admin | Full administrative access to TWC | `<AD_GRP_TWC_ADMIN>` |
| TWC-ProjectAdmin | Create/manage projects, manage members | `<AD_GRP_TWC_PROJADMIN>` |
| TWC-Modeller | Read/write access to assigned projects | `<AD_GRP_TWC_MODELLER>` |
| TWC-Reviewer | Read-only access to assigned projects | `<AD_GRP_TWC_REVIEWER>` |
| TWC-AuditViewer | Access to audit logs only | `<AD_GRP_TWC_AUDIT>` |

---

### Permission Matrix (TWC)

| Permission | TWC-Admin | TWC-ProjectAdmin | TWC-Modeller | TWC-Reviewer | TWC-AuditViewer |
|-----------|:---------:|:----------------:|:------------:|:------------:|:---------------:|
| Create project | ✓ | ✓ | ✗ | ✗ | ✗ |
| Delete project | ✓ | ✗ | ✗ | ✗ | ✗ |
| Add project member | ✓ | ✓ | ✗ | ✗ | ✗ |
| Commit changes | ✓ | ✓ | ✓ | ✗ | ✗ |
| Read project | ✓ | ✓ | ✓ | ✓ | ✗ |
| View audit logs | ✓ | ✗ | ✗ | ✗ | ✓ |
| Manage users | ✓ | ✗ | ✗ | ✗ | ✗ |
| Server configuration | ✓ | ✗ | ✗ | ✗ | ✗ |

> ⚠️ **Status:** This document is Not Started. AD group names require confirmation.

---

## FLEXNET — FlexNet License Server

> **Source:** `flexnet/docs/08_rbac_definition.md` | **Status:** ✅ Complete | **Version:** 0.2.0

---

### Roles Summary (FlexNet)

| Role ID | Role Name | Scope | AD Group |
|---------|-----------|-------|---------|
| FNA | FlexNet Administrator | Full lmadmin access; service management via OS sudo | `<AD_GROUP_FLEXNET_ADMIN>` |
| FNO | FlexNet Operator | Read-only lmadmin; log access; no service control | `<AD_GROUP_FLEXNET_OPERATOR>` |
| FNT | FlexNet Auditor | Audit log read access only | `<AD_GROUP_FLEXNET_AUDITOR>` |
| OSA | OS Administrator | RHEL 9 OS management via sudo | `<AD_GROUP_OS_ADMIN>` |
| SVC | Service Account | Non-interactive; runs `flexnet.service` only | `svc_flexnet` (local system) |

---

### Privilege Matrix (FlexNet)

| Action | FNA | FNO | FNT | OSA | SVC |
|--------|:---:|:---:|:---:|:---:|:---:|
| View licence checkout status (lmstat) | ✅ | ✅ | ❌ | ✅ | N/A |
| View audit logs | ✅ | ✅ | ✅ | ✅ | N/A |
| Start / stop / restart flexnet.service | ✅ | ❌ | ❌ | ✅ | N/A |
| Force licence reload (lmreread) | ✅ | ❌ | ❌ | ❌ | N/A |
| Remove stuck checkout (lmremove) | ✅ | ❌ | ❌ | ❌ | N/A |
| Deploy / replace licence file | ✅ | ❌ | ❌ | ❌ | N/A |
| lmadmin web console — full admin | ✅ | ❌ | ❌ | ❌ | N/A |
| lmadmin web console — read-only | ✅ | ✅ | ❌ | ❌ | N/A |
| OS sudo (full) | ❌ | ❌ | ❌ | ✅ | N/A |
| Apply OS patches | ❌ | ❌ | ❌ | ✅ | N/A |
| Run `flexnet.service` process | ❌ | ❌ | ❌ | ❌ | ✅ |
| Interactive OS login | ❌ | ❌ | ❌ | ✅ | ❌ |

---

### Filesystem ACLs (FlexNet)

```bash
# FlexNet installation directory
sudo chown -R root:svc_flexnet /opt/flexnet && sudo chmod -R 750 /opt/flexnet

# Licence file — readable by service account; not world-readable
sudo chown root:svc_flexnet /etc/flexnet/license.lic && sudo chmod 640 /etc/flexnet/license.lic

# TLS private key — root-only read
sudo chown root:root /etc/flexnet/tls/server.key && sudo chmod 400 /etc/flexnet/tls/server.key

# Log directory
sudo chown -R svc_flexnet:svc_flexnet /var/log/flexnet && sudo chmod -R 750 /var/log/flexnet
```

---

## CST — Cameo Simulation Toolkit (CST)

> **Source:** `cst/docs/08_rbac_definition.md` | **Status:** In Progress 0.2-DRAFT | **Doc Ref:** DOC-08

---

### Role Definitions (CST)

| Role | Description | Assigned Persona | Granted By |
|------|-------------|-----------------|-----------|
| `CST_USER` | Run simulations locally within CSM; view and export own results | Systems Engineers | MBSE Tool Administrator |
| `CST_SIMULATION_SPECIALIST` | Run advanced simulations; delegate to server-side; configure model execution parameters | Simulation Specialists | MBSE Tool Administrator |
| `CST_ADMIN` | Install and update CST plugin; configure JVM and licence settings; view audit logs | MBSE Tool Administrators | Platform Operations |
| `CST_PLATFORM_OPS` | Manage server-side CST service on Windows Server 2025; apply OS hardening; manage AD groups | Platform Operations | Programme Security Authority |
| `CST_READONLY` | View simulation results only; no simulation execution rights | Reviewers / Auditors | MBSE Tool Administrator |

---

### Permission Matrix (CST)

| Permission | CST_USER | CST_SIMULATION_SPECIALIST | CST_ADMIN | CST_PLATFORM_OPS | CST_READONLY |
|------------|:--------:|:------------------------:|:---------:|:---------------:|:------------:|
| Run local simulation (CSM client) | ✅ | ✅ | ✅ | ❌ | ❌ |
| Run server-side simulation (via WAP) | ❌ | ✅ | ✅ | ❌ | ❌ |
| View own simulation results | ✅ | ✅ | ✅ | ❌ | ❌ |
| View all simulation results | ❌ | ❌ | ✅ | ✅ | ✅ |
| Export own results (CSV/XML/PDF) | ✅ | ✅ | ✅ | ❌ | ✅ |
| Delete simulation results | ❌ | ❌ | ✅ | ❌ | ❌ |
| Configure JVM settings (client) | ❌ | ❌ | ✅ | ❌ | ❌ |
| Configure JVM settings (server) | ❌ | ❌ | ❌ | ✅ | ❌ |
| Install / update CST plugin (client) | ❌ | ❌ | ✅ | ❌ | ❌ |
| Deploy / restart CST server service | ❌ | ❌ | ❌ | ✅ | ❌ |
| View Windows Event Log (simulation) | ❌ | ❌ | ✅ | ✅ | ❌ |
| Manage AD group membership | ❌ | ❌ | ❌ | ✅ | ❌ |

---

## CSM — Cameo Systems Modeler (CSM)

> **Source:** `csm/docs/08_rbac_definition.md` | **Status:** ✅ Done

---

### Role Definitions (CSM)

| Role ID | Role Name | Description |
|---|---|---|
| R01 | Systems Engineer | Primary SysML modelling user |
| R02 | Lead Architect | Senior modeller; model review and approval |
| R03 | MBSE Tool Administrator | Manages CSM package, plugins, JVM config |
| R04 | VDI Platform Engineer | Manages Horizon pool, OS image, profiles |
| R05 | Cybersecurity Compliance Officer | Reviews compliance posture; approves exceptions |

---

### Access Matrix (CSM)

**VDI Access:**

| Permission | R01 | R02 | R03 | R04 | R05 |
|---|---|---|---|---|---|
| Connect to Horizon VDI | ✅ | ✅ | ✅ | ✅ | 🔍 Audit only |
| Local Administrator on VDI | ❌ | ❌ | ⚠️ Break-glass only | ✅ | ❌ |
| Reset VDI pool desktop | ❌ | ❌ | ❌ | ✅ | ❌ |

**CSM Application:**

| Permission | R01 | R02 | R03 | R04 | R05 |
|---|---|---|---|---|---|
| Launch CSM | ✅ | ✅ | ✅ | ❌ | ❌ |
| Create / edit SysML models | ✅ | ✅ | ❌ | ❌ | ❌ |
| Approve / baseline models | ❌ | ✅ | ❌ | ❌ | ❌ |
| Install / update plugins | ❌ | ❌ | ✅ | ❌ | ❌ |
| Modify JVM configuration | ❌ | ❌ | ✅ | ❌ | ❌ |

**Licence Server:**

| Permission | R01 | R02 | R03 | R04 | R05 |
|---|---|---|---|---|---|
| Consume floating licence | ✅ | ✅ | ✅ | ❌ | ❌ |
| View licence usage | ❌ | ❌ | ✅ | ❌ | ✅ |
| Administer licence server | ❌ | ❌ | ✅ | ❌ | ❌ |

---

### AD Group Mapping (CSM)

| Role | AD Group | Notes |
|---|---|---|
| R01 — Systems Engineer | `<AD_GROUP_SE>` | Grants Horizon pool entitlement; FlexNet licence seat |
| R02 — Lead Architect | `<AD_GROUP_LA>` | Grants Horizon pool entitlement; FlexNet licence seat; TWC project create rights |
| R03 — MBSE Tool Admin | `<AD_GROUP_ADMIN>` | Grants Horizon admin console access; Numecent admin access |
| R04 — VDI Platform Engineer | `<AD_GROUP_VDI>` | Grants Horizon infrastructure admin rights |
| R05 — Compliance Officer | `<AD_GROUP_COMPLIANCE>` | Read-only audit access |

---

*Generated: 2026-04-08 | Classification: OFFICIAL — SENSITIVE | Author: Iain Reid*
