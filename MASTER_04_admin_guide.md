# Phoenix CAMEO — Master Administrator Guide

> **Programme:** Phoenix CAMEO MBSE  
> **Document Type:** Administrator Guide  
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

> **Source:** `wap/docs/04_administrator_guide.md` | **Status:** Draft 0.2 | **Doc Ref:** WAP-DOC-04

# WAP-DOC-04 — Administrator Guide

**Audience:** Platform administrators with SSH access to the WAP VM.

---

### 1. Service Management

All WAP services are managed via systemd. Services must be started and stopped in dependency order.

| Service Unit | Managed Component | Start Order |
|---|---|---|
| `wap-core.service` | WAP Core JVM | 1st |
| `wap-resources.service` | Resources Application | 2nd |
| `wap-collaborator.service` | Cameo Collaborator | 3rd |
| `wap-doc-exporter.service` | Document Exporter | 4th |
| `wap-cst.service` | CST Simulation Services | 5th |

**Start All Services:**
```bash
sudo systemctl start wap-core.service
sudo systemctl start wap-resources.service
sudo systemctl start wap-collaborator.service
sudo systemctl start wap-doc-exporter.service
sudo systemctl start wap-cst.service
```

**Stop All Services (reverse order):**
```bash
sudo systemctl stop wap-cst.service
sudo systemctl stop wap-doc-exporter.service
sudo systemctl stop wap-collaborator.service
sudo systemctl stop wap-resources.service
sudo systemctl stop wap-core.service
```

---

### 2. TLS Certificate Management

**Check Certificate Expiry:**
```bash
openssl pkcs12 -in /opt/nomagic/wap/conf/ssl/keystore.p12 \
  -nokeys -passin pass:<keystore-password> \
  | openssl x509 -noout -dates
```

**Threshold:** Alert when certificate expiry is within 60 days.

**Renewal Procedure:**
1. Request a new certificate from the internal CA for the WAP hostname.
2. Export as PKCS#12 including the full chain.
3. Copy to `/opt/nomagic/wap/conf/ssl/keystore.p12` (backup the old file first).
4. Restart WAP Core: `sudo systemctl restart wap-core.service`
5. Verify with a browser that the new certificate is served and trusted.

---

### 3. Active Directory Integration

**Test LDAPS Connectivity:**
```bash
ldapsearch -H ldaps://<AD-hostname>:636 \
  -D "cn=svc-wap-bind,ou=ServiceAccounts,dc=domain,dc=local" \
  -w <bind-password> \
  -b "ou=Users,dc=domain,dc=local" \
  "(sAMAccountName=testuser)"
```

---

### 4. Health Checks

| Check | Command / URL | Expected Result |
|---|---|---|
| WAP web UI accessible | `curl -sk https://<wap-hostname>/` | HTTP 200 |
| WAP admin console | `curl -sk https://<wap-hostname>:8443/admin/` | HTTP 200 |
| WAP Core process running | `systemctl is-active wap-core.service` | `active` |
| TWC reachable from WAP | `curl -k -u svc-wap-twc:<password> https://<twc>:8111/osmc/ping` | HTTP 200 |

---

### 5. Backup Scope

The WAP VM is stateless — model data is owned by TWC. WAP-local items to back up:

| Item | Location | Frequency |
|---|---|---|
| Configuration files | `/opt/nomagic/wap/conf/` | Before any change |
| TLS keystore and truststore | `/opt/nomagic/wap/conf/ssl/` | Before any change |
| systemd unit files | `/etc/systemd/system/wap-*.service` | Before any change |

---

## TWC — Teamwork Cloud (TWC)

> **Source:** `twc/docs/04_administrator_guide.md` | **Status:** Not Started 0.1-DRAFT | **Doc Ref:** DOC-04

# DOC-04 — Administrator Guide
## Teamwork Cloud Core Repository VM

**Audience:** MBSE Tool Administrators, Cybersecurity Operations

---

### 1. Service Management

**Start/Stop Order:** Zookeeper → Cassandra → TWC

```bash
# Start
systemctl start zookeeper
systemctl start cassandra
systemctl start twc

# Stop
systemctl stop twc
systemctl stop cassandra
systemctl stop zookeeper
```

### 2. Health Checks

| Check | Command / Method | Expected Result |
|-------|-----------------|-----------------|
| TWC API | `curl -k https://localhost:8111/` | HTTP 200 |
| Cassandra | `nodetool status` | UN (Up/Normal) for all nodes |
| Zookeeper | `echo ruok \| nc localhost 2181` | `imok` |

### 3. Log Locations

| Service | Log Path |
|---------|---------|
| TWC | `/opt/twc/logs/` |
| Cassandra | `/var/log/cassandra/` |
| Zookeeper | `/var/log/zookeeper/` |
| OS audit | `/var/log/audit/audit.log` |

> ⚠️ **Status:** This document is Not Started. Sections require completion.

---

## FLEXNET — FlexNet License Server

> **Source:** `flexnet/docs/03_administrator_guide.md` | **Status:** ✅ Complete | **Version:** 0.2.0

# 03 — Administrator Guide

**Audience:** FlexNet Administrators | **Classification:** OFFICIAL — SENSITIVE

---

### 1. Prerequisites

| Requirement | Detail |
|-------------|--------|
| AD group membership | Member of `<AD_GROUP_FLEXNET_ADMIN>` (Full) or `<AD_GROUP_FLEXNET_OPERATOR>` (Read-only) |
| OS access | SSH access to `<FLEXNET_FQDN>` via `<AD_GROUP_OS_ADMIN>` |
| Tools | `lmutil` available at `/opt/flexnet/bin/lmutil` |
| Licence file path | `/etc/flexnet/license.lic` |

---

### 2. Service Management

```bash
# Start the service
sudo systemctl start flexnet.service

# Stop the service (disconnects all active licence checkouts)
sudo systemctl stop flexnet.service

# Restart
sudo systemctl restart flexnet.service

# View current status
sudo systemctl status flexnet.service
```

> **Warning:** `systemctl stop` immediately terminates all active licence checkouts. Use `lmutil lmreread` where possible to reload without interrupting active sessions.

**Reload Licence File Without Restart:**
```bash
/opt/flexnet/bin/lmutil lmreread -c /etc/flexnet/license.lic
```

---

### 3. Licence Operations

```bash
# Check all licence status
/opt/flexnet/bin/lmutil lmstat -a -c /etc/flexnet/license.lic

# Check checkouts for a specific feature
/opt/flexnet/bin/lmutil lmstat -f <FEATURE_NAME> -c /etc/flexnet/license.lic

# Force-release a stuck checkout
/opt/flexnet/bin/lmutil lmremove -c /etc/flexnet/license.lic <FEATURE_NAME> <USER> <HOST> <DISPLAY>

# Verify licence expiry dates
/opt/flexnet/bin/lmutil lmstat -a -c /etc/flexnet/license.lic | grep -E "(Users of|expires)"
```

---

### 4. lmadmin Web Interface

| Item | Value |
|------|-------|
| URL | `https://<FLEXNET_FQDN>:8090` |
| Authentication | Active Directory — use your named AD account |
| Certificate | Issued by internal CA |

**ISSUE-003:** Rotating lmadmin default credentials is **mandatory on day 1 post-installation.** Navigate to Administration → Change Password.

---

### 5. Log Management

| Log | Path | Contents |
|-----|------|---------|
| lmgrd daemon log | `/var/log/flexnet/lmgrd.log` | Service start/stop, checkout/checkin events, errors |
| lmadmin log | `/var/log/flexnet/lmadmin.log` | Web console access, admin actions |
| OS audit log | `/var/log/audit/audit.log` | SELinux, sudo, login events |

```bash
# Live tail
sudo tail -f /var/log/flexnet/lmgrd.log

# Search for errors
sudo grep -i "error\|denied\|cannot" /var/log/flexnet/lmgrd.log | tail -50
```

---

### 6. Quick Health Check

```bash
echo "=== Service Status ==="
sudo systemctl status flexnet.service --no-pager

echo "=== Licence Status ==="
/opt/flexnet/bin/lmutil lmstat -a -c /etc/flexnet/license.lic

echo "=== FIPS Mode ==="
fips-mode-setup --check

echo "=== SELinux ==="
getenforce

echo "=== Port Listeners ==="
ss -tlnp | grep -E "27000|8090"
```

---

## CST — Cameo Simulation Toolkit (CST)

> **Source:** `cst/docs/04_administrator_configuration_guide.md` | **Status:** In Progress 0.2-DRAFT | **Doc Ref:** DOC-04

# DOC-04 — Administrator Configuration Guide

**Audience:** MBSE Tool Administrators and Platform Operations personnel.

---

### 1. Client-Side Configuration (Windows 10/11)

**JVM Configuration** — edit `<CSM_INSTALL_DIR>\bin\csm.vmoptions`:

| Parameter | Purpose | Recommended Value |
|-----------|---------|------------------|
| `-Xms` | Initial JVM heap | `512m` |
| `-Xmx` | Maximum JVM heap | `2048m` |
| `-XX:+UseG1GC` | Garbage collector | Enable |
| `-Djava.security.properties` | FIPS crypto policy | Point to FIPS policy file |

**Licence Configuration** — copy and populate `config/licence.properties.template`:

| Property | Value |
|----------|-------|
| `flexnet.server.host` | `<REPLACE_ME_FLEXNET_HOSTNAME>` |
| `flexnet.server.port` | `<REPLACE_ME_FLEXNET_PORT>` |
| `flexnet.feature` | `<REPLACE_ME_CST_FEATURE_NAME>` |

**Verify licence connectivity:**
```powershell
Test-NetConnection -ComputerName <REPLACE_ME_FLEXNET_HOSTNAME> -Port <REPLACE_ME_FLEXNET_PORT>
# Expected: TcpTestSucceeded : True
```

---

### 2. FIPS 140-3 Cryptography Mode (Server-Side)

Enable via Group Policy:
Path: `Computer Configuration → Windows Settings → Security Settings → Local Policies → Security Options`  
Policy: `System cryptography: Use FIPS compliant algorithms` → **Enabled**

Verify:
```powershell
Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\FipsAlgorithmPolicy" -Name Enabled
# Expected: Enabled = 1
```

---

### 3. Hardening Checklist

| Check | Standard | Verification |
|-------|----------|-------------|
| FIPS 140-3 mode enabled | FIPS 140-3 | `scripts/verify_fips.ps1` |
| TLS 1.0 / 1.1 disabled | CIS L2 | `scripts/verify_deployment.ps1` |
| SMBv1 disabled | CIS L2 / DISA STIG | `Get-SmbServerConfiguration \| Select EnableSMB1Protocol` |
| Windows Firewall enabled | DISA STIG | `Get-NetFirewallProfile \| Select Name,Enabled` |
| Audit policy configured | NIST AU-2 / DISA STIG | `auditpol /get /category:*` |
| Local administrator disabled | CIS L2 | `Get-LocalUser` |

---

## CSM — Cameo Systems Modeler (CSM)

> **Source:** `csm/docs/04_administrator_support_guide.md` | **Status:** ✅ Done

# 04 — Administrator / Support Guide

**Audience:** MBSE Tool Administrators, VDI Platform Engineers, Cybersecurity Compliance Officers

---

### 1. VDI Pool Management

The CSM deployment uses a **dedicated persistent VDI pool** within VMware Horizon.

**Adding Users to the Pool:**
1. In the Horizon Administrator Console, navigate to **Inventory → Desktops → [Pool Name]**.
2. Select **Entitlements → Add Entitlement**.
3. Search for and add the user's AD account or AD group.

**Resetting a VDI:**

> **Warning:** Never use **Horizon Delete and Recreate** on a persistent pool desktop unless data has been backed up or all models committed to Teamwork Cloud.

For a soft reset (reboot only), right-click the desktop → **Reset** (not Delete).

---

### 2. Numecent Package Management

**Publishing a New Package Version:**
1. Validate package integrity: `python scripts\validate_package.py --package <PACKAGE_FILE> --expected-hash <SHA256>`
2. Log in to the Numecent Cloudpaging Administrator Console.
3. Navigate to **Applications → [CSM Application] → Upload New Version**.
4. Set the new version as **Active** only after testing on at least one test VDI.

**Rolling Back:**
In the Numecent Cloudpaging Administrator Console, navigate to **Applications → [CSM Application]**, select the previous version from history, and set it as **Active**.

---

### 3. Licence Administration

| Task | How |
|---|---|
| View current licence usage | FlexNet/DSLS admin console → Licence Usage dashboard |
| Reclaim a stuck licence | FlexNet console → identify orphaned checkout → remove if session confirmed dead |
| Confirm licence server connectivity | `python scripts\health_check.py` |

---

### 4. OS Patching

Monthly cadence aligned with Patch Tuesday. Maximum patch lag: **Patch Tuesday + 14 calendar days** for Critical/High severity patches.

1. Snapshot the current VDI gold image.
2. Apply patches via WSUS (no direct internet access).
3. Run STIG/CIS re-validation: `.\scripts\harden_vdi.ps1 -Mode Validate`
4. Launch CSM and confirm licence checkout succeeds.
5. Seal the updated image and create a new snapshot tagged with the patch date.

---

*Generated: 2026-04-08 | Classification: OFFICIAL — SENSITIVE | Author: Iain Reid*
