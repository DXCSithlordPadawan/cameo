# Phoenix CAMEO — Master Compliance Mapping

> **Programme:** Phoenix CAMEO MBSE  
> **Document Type:** Compliance Mapping  
> **Generated:** 2026-04-08  
> **Components Covered:** WAP · TWC · FlexNet · CST · CSM

---

## Applicable Standards (All Components)

| Standard | Version | Scope |
|---|---|---|
| NIST SP 800-53 | Rev 5 | Security and privacy controls |
| OWASP Top 10 | 2021 | Web application threat categories |
| DISA STIG | RHEL 9 V1R1 / Windows Server 2025 V1R1 | OS and application hardening |
| CIS Benchmark | Level 2 — RHEL 9 / Windows Server 2025 | Host hardening |
| FIPS 140-3 | Current | Cryptographic module validation |
| JSP 939 | Current | Defence policy for modelling and simulation |
| JSP 936 Part 1 | Current | Modelling and simulation safety |

**Sources:**
- NIST SP 800-53 Rev 5: https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final
- OWASP Top 10 2021: https://owasp.org/Top10/
- DISA STIG Library: https://public.cyber.mil/stigs/
- CIS Benchmarks: https://www.cisecurity.org/cis-benchmarks/
- NIST CMVP: https://csrc.nist.gov/projects/cryptographic-module-validation-program
- JSP 939: https://www.gov.uk/government/publications/defence-policy-for-modelling-and-simulation-jsp-939
- JSP 936 Part 1: https://assets.publishing.service.gov.uk/media/6735fc89f6920bfb5abc7b62/JSP936_Part1.pdf

---

## Contents

- [WAP — Web Application Platform (WAP)](#wap--web-application-platform-wap)
- [TWC — Teamwork Cloud (TWC)](#twc--teamwork-cloud-twc)
- [FLEXNET — FlexNet License Server](#flexnet--flexnet-license-server)
- [CST — Cameo Simulation Toolkit (CST)](#cst--cameo-simulation-toolkit-cst)
- [CSM — Cameo Systems Modeler (CSM)](#csm--cameo-systems-modeler-csm)

---

## WAP — Web Application Platform (WAP)

> **Source:** `wap/docs/07_compliance_mapping.md` | **Status:** Draft 0.2 | **Doc Ref:** WAP-DOC-07

---

### NIST SP 800-53 Rev 5 — WAP Control Mapping

| Control ID | Control Name | WAP Implementation | Status |
|---|---|---|---|
| AC-2 | Account Management | All accounts managed via Active Directory. No local WAP accounts permitted. | ⬜ Verify |
| AC-3 | Access Enforcement | RBAC enforced at WAP API middleware for every endpoint | ⬜ Verify |
| AC-6 | Least Privilege | WAP runs as non-root `wap` service account | ⬜ Verify |
| AU-2 | Event Logging | All authentication, authorisation, admin, and job submission events logged | ⬜ Verify |
| AU-9 | Protection of Audit Information | Logs forwarded to SIEM in near-real-time; local logs `0640` owned by `wap` | ⬜ Verify |
| CA-7 | Continuous Monitoring | Health checks automated; TLS expiry monitored; dependency CVE scanning monthly | ⬜ Verify |
| CM-7 | Least Functionality | Only required services running; unused ports closed; firewalld default-deny | ⬜ Verify |
| IA-2 | Identification and Authentication | AD-backed authentication via LDAPS; no anonymous access | ⬜ Verify |
| RA-3 | Risk Assessment | Threat model completed; residual risks documented | ✅ Draft |
| SC-8 | Transmission Confidentiality | TLS 1.2+ enforced on all endpoints; mTLS to TWC | ⬜ Verify |
| SC-13 | Cryptographic Protection | FIPS 140-3 validated JVM and TLS modules in use | ⬜ Verify |
| SC-28 | Protection of Information at Rest | No model data stored locally; config files `0640`; SSL keys `0700` | ⬜ Verify |
| SI-2 | Flaw Remediation | Monthly OS and application patch review; CVE scanning | ⬜ Verify |

---

### FIPS 140-3 Cryptographic Compliance (WAP)

**Approved TLS 1.2 cipher suites:**
```
TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
```

**Prohibited:** RC4, DES, 3DES, MD5, TLS 1.0, TLS 1.1, SSLv2, SSLv3, export-grade ciphers.

---

## TWC — Teamwork Cloud (TWC)

> **Source:** `twc/docs/07_compliance_mapping.md` | **Status:** Not Started 0.1-DRAFT | **Doc Ref:** DOC-07

---

### NIST SP 800-53 Mapping (TWC)

| Control ID | Control Name | Implementation | Status |
|-----------|-------------|---------------|--------|
| AC-2 | Account Management | AD-based user lifecycle | Not Verified |
| AC-3 | Access Enforcement | TWC RBAC + AD groups | Not Verified |
| AU-2 | Audit Events | TWC + OS audit logging | Not Verified |
| IA-2 | Identification and Authentication | AD + mTLS | Not Verified |
| SC-7 | Boundary Protection | Egress firewall deny-all | Not Verified |
| SC-8 | Transmission Confidentiality | mTLS (FIPS-compliant cipher) | Not Verified |
| SC-28 | Protection of Information at Rest | OS-level encryption | Not Verified |
| SI-2 | Flaw Remediation | Patch management process | Not Verified |

### FIPS 140-3 (TWC)

| Requirement | Implementation | Status |
|------------|---------------|--------|
| FIPS-validated crypto module | OpenSSL 3.x FIPS provider | Not Verified |
| Approved cipher suites only | TLS 1.2/1.3 with approved suites | Not Verified |
| JVM FIPS mode | JVM startup flag | Not Verified |

> ⚠️ **Status:** This document is Not Started. Full mapping to be completed.

---

## FLEXNET — FlexNet License Server

> **Source:** `flexnet/docs/07_compliance_mapping.md` | **Status:** ✅ Complete | **Version:** 0.2.0

---

### NIST SP 800-53 Rev 5 (FlexNet)

| Control ID | Control Name | Implementation | Status |
|-----------|--------------|----------------|--------|
| AC-2 | Account Management | AD-backed accounts; `svc_flexnet` system account only; quarterly account review | ⬜ Planned |
| AC-6 | Least Privilege | `svc_flexnet` non-interactive; restricted sudo for OS admins | ⬜ Planned |
| AU-2 | Event Logging | lmgrd checkout/checkin events; lmadmin admin actions; auditd OS events | ⬜ Planned |
| AU-11 | Audit Record Retention | 90-day on-host retention via logrotate; SIEM for long-term | ⬜ Planned |
| CM-2 | Baseline Configuration | `config/deployment.yaml` as single source of truth | ✅ Implemented |
| CP-10 | System Recovery | Snapshot-based recovery; runbook in docs/05; RTO 4h (draft) | ✅ Implemented (doc) |
| IR-4 | Incident Handling | Incident response triggers in docs/05; escalation path documented | ✅ Implemented (doc) |
| SC-8 | Transmission Confidentiality and Integrity | TLS 1.2+ for lmadmin; LDAPS for AD; checkout protocol plaintext (GAP-005 — residual) | ⚠ Partial |
| SI-2 | Flaw Remediation | Monthly OS patch cycle; FlexNet Publisher updates per doc 10 | ✅ Implemented (doc) |

---

### FIPS 140-3 (FlexNet)

| Requirement | Implementation | Status |
|-------------|----------------|--------|
| Kernel FIPS mode enabled | `fips-mode-setup --enable` on RHEL 9 | ⬜ Planned (ISSUE-002) |
| OpenSSL FIPS provider | RHEL 9 ships OpenSSL with FIPS provider | ⬜ Planned |
| FlexNet Publisher FIPS compatibility | **ISSUE-002 — lab validation required.** | ❌ Open |
| TLS cipher suites for lmadmin | TLS_AES_256_GCM_SHA384; ECDHE-RSA-AES256-GCM-SHA384 | ⚠ Partial |

---

### JSP 939 and JSP 936 Alignment (FlexNet)

| Requirement | Alignment |
|-------------|-----------|
| JSP 939 — Modelling and Simulation governance | The licence server is a dependency of all CAMEO tools within the JSP 939 scope. |
| JSP 936 Part 1 — Software governance | FlexNet Publisher is COTS software; procurement via IBM Passport Advantage satisfies JSP 936 supply chain requirements. |

---

## CST — Cameo Simulation Toolkit (CST)

> **Source:** `cst/docs/07_compliance_mapping.md` | **Status:** In Progress 0.2-DRAFT | **Doc Ref:** DOC-07

---

### NIST SP 800-53 Rev 5 (CST)

| Control Family | Control ID | Control Name | Implementation | Status |
|----------------|------------|-------------|---------------|--------|
| Access Control | AC-2 | Account Management | All CST user accounts managed via AD | 🔲 Pending |
| Access Control | AC-3 | Access Enforcement | RBAC roles enforced at WAP gateway and NTFS result store | 🔲 Pending |
| Access Control | AC-6 | Least Privilege | Simulation execution bounded to invoking user's OS privileges | 🔲 Pending |
| Audit | AU-2 | Event Logging | All simulation launches, results, errors logged to Windows Event Log | 🔲 Pending |
| Config Mgmt | CM-6 | Configuration Settings | `scripts/harden_server.ps1` — CIS L2 + DISA STIG | 🔲 Pending |
| Config Mgmt | CM-7 | Least Functionality | Unnecessary services disabled via harden_server.ps1 | 🔲 Pending |
| Sys Comms | SC-8 | Transmission Confidentiality | TLS 1.2+ on all network interfaces | 🔲 Pending |
| Sys & Info | SI-2 | Flaw Remediation | Windows Update / WSUS; vendor patch process | 🔲 Pending |
| Sys & Info | SI-3 | Malicious Code Protection | Windows Defender enabled and updated | 🔲 Pending |

---

### FIPS 140-3 (CST)

| Item | Requirement | Verification |
|------|-------------|-------------|
| OS FIPS mode | Enabled on Windows Server 2025 via Group Policy | `scripts/verify_fips.ps1` |
| JVM cryptographic provider | Must use FIPS-validated module | Confirm with vendor (GAP-01) |
| TLS cipher suites | Only FIPS-approved suites | `config/tls_policy.reg.template` |
| Key lengths | RSA ≥ 2048-bit; AES ≥ 128-bit | PKI audit |

---

### JSP 939 / JSP 936 (CST)

| Document | Applicability |
|----------|--------------|
| JSP 939 | CST is a simulation tool in defence MBSE context. CST simulation outputs must be clearly identified as simulation artefacts, not operational data. Determinism requirement supports JSP 939 fidelity and rigour requirements. |
| JSP 936 Part 1 | Simulation outputs influence design decisions. Results must not be used as primary safety evidence without formal V&V. Results must be reproducible and auditable per JSP 936 requirements. |

---

## CSM — Cameo Systems Modeler (CSM)

> **Source:** `csm/docs/07_compliance_mapping.md` | **Status:** ✅ Done

---

### NIST SP 800-53 Rev 5 (CSM)

| Control | Title | Implementation | Evidence Location |
|---|---|---|---|
| AC-2 | Account Management | AD-based accounts; no local CSM accounts | `MASTER_08_rbac_definition.md` |
| AC-6 | Least Privilege | User role — model authoring only; no admin rights | `MASTER_08_rbac_definition.md` |
| AU-2 | Event Logging | VDI OS audit policy; FlexNet licence audit logs | `MASTER_04_admin_guide.md` |
| IA-2 | Identification & Authentication | Kerberos AD authentication | `MASTER_05_api_integration_guide.md` |
| IA-5 | Authenticator Management | No embedded credentials in package or config | `MASTER_06_security_analysis.md` |
| SC-7 | Boundary Protection | No internet; allow-list firewall rules | `MASTER_05_api_integration_guide.md` |
| SC-28 | Protection of Information at Rest | FIPS 140-3 disk encryption on VDI | `MASTER_12_deployment_guide.md` |
| SI-3 | Malicious Code Protection | AV with approved exclusions | `MASTER_04_admin_guide.md` |
| SI-7 | Software, Firmware & Information Integrity | Numecent signed package; integrity check at launch | `scripts/validate_package.py` |

---

### FIPS 140-3 Coverage (CSM)

| Module | Location | FIPS 140-3 Status | Certificate # |
|---|---|---|---|
| Windows CNG | VDI OS | FIPS 140-3 validated | Confirm via NIST CMVP |
| BitLocker | VDI OS | Uses Windows CNG | Confirm with NIST CMVP |
| JVM cryptographic provider | Numecent package (JVM) | Pending confirmation — open item | TBC with Dassault |
| TLS stack for TWC | VDI OS / JVM | TLS 1.2 minimum; FIPS cipher suites enforced | Confirm cipher suite list |

**FIPS mode activation:** `Computer Configuration → Windows Settings → Security Settings → Local Policies → Security Options → System cryptography: Use FIPS compliant algorithms` → **Enabled**

---

### JSP 939 / JSP 936 Notes (CSM)

**JSP 939:** CSM is designated as a MBSE/SysML authoring tool. The Numecent versioned package approach satisfies the configuration control requirement. Programme authority approval of the CSM release version is required before deployment.

**JSP 936 Part 1:** CSM must be registered with the programme M&S governance board as an approved tool. Model outputs produced using CSM should be subject to V&V review by the Lead Architect before use in downstream M&S or system design activities.

> **References:**
> - JSP 939: https://www.gov.uk/government/publications/defence-policy-for-modelling-and-simulation-jsp-939
> - JSP 936 Part 1: https://assets.publishing.service.gov.uk/media/6735fc89f6920bfb5abc7b62/JSP936_Part1.pdf

---

*Generated: 2026-04-08 | Classification: OFFICIAL — SENSITIVE | Author: Iain Reid*
