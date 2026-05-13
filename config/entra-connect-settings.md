# ⚙️ Microsoft Entra Connect — Configuration Reference

> Full configuration reference for the Microsoft Entra Connect deployment
> in the `azure-hybrid-identity-lab` project.
> Entra Connect is installed on `VM-WINSERV-01` (`192.168.1.10`).

---

## 🌐 Environment

| Item | Value |
|------|-------|
| **Installed On** | `VM-WINSERV-01` — `192.168.1.10` |
| **On-Premises Domain** | `InfoTech.com` |
| **Azure Tenant** | `yourtenant.onmicrosoft.com` |
| **Sync Service** | `ADSync` (Windows Service) |
| **Sync Interval** | Every 30 minutes (delta sync) |
| **Sync Direction** | On-premises → Cloud (one-way) |
| **Sync Method** | Password Hash Synchronization |

---

## 🔧 Installation Settings

| Setting | Value |
|---------|-------|
| **Installation Type** | Custom (not Express) |
| **Sign-in Method** | Password Hash Synchronization |
| **Source Anchor** | Managed by Azure (default) |
| **UPN Matching** | Continued without match — `InfoTech.com` is unverified |
| **Password Writeback** | Enabled — required for SSPR |
| **Device Writeback** | Disabled |
| **Exchange Hybrid** | Disabled |

---

## 🗂️ Sync Scope — OU Filtering

Only the `Azure_Sync` OU is included in the sync scope.
All other OUs are excluded.

| OU | Sync Status |
|----|------------|
| `OU=Azure_Sync,DC=InfoTech,DC=com` | ✅ Included |
| `OU=All_Staff,DC=InfoTech,DC=com` | ❌ Excluded |
| `OU=Disabled_Users,DC=InfoTech,DC=com` | ❌ Excluded |
| `CN=Users,DC=InfoTech,DC=com` | ❌ Excluded |

---

## 👤 Synced Users

| On-Premises UPN | Cloud UPN | Department | Source |
|-----------------|-----------|-----------|--------|
| `paula@InfoTech.com` | `paula@yourtenant.onmicrosoft.com` | IT | Windows Server AD |
| `dave@InfoTech.com` | `dave@yourtenant.onmicrosoft.com` | IT | Windows Server AD |
| `sue@InfoTech.com` | `sue@yourtenant.onmicrosoft.com` | IT | Windows Server AD |
| `rdoe@InfoTech.com` | `rdoe@yourtenant.onmicrosoft.com` | HR | Windows Server AD |

---

## 🔑 Service Account

| Item | Value |
|------|-------|
| **Account Name** | `EntConnSync` |
| **UPN** | `EntConnSync@InfoTech.com` |
| **Password Expires** | Never (lab setting) |
| **Purpose** | Entra Connect reads AD objects via this account |

---

## 🔄 Sync Management Commands

```powershell
# Import the ADSync module
Import-Module ADSync

# Check sync scheduler status
Get-ADSyncScheduler

# Force a full sync (use after major AD changes)
Start-ADSyncSyncCycle -PolicyType Initial

# Force a delta sync (use for small changes)
Start-ADSyncSyncCycle -PolicyType Delta

# Check sync service status
Get-Service -Name "ADSync"

# Restart sync service if needed
Restart-Service -Name "ADSync"

# View sync errors
Get-ADSyncConnectorStatistics -ConnectorName "InfoTech.com"
```

---

## 📋 Sync Log Location

```
C:\ProgramData\AADConnect\trace-*.log
Event Viewer → Applications and Services Logs → Directory Synchronization
```

---

## ⚠️ Known Behaviours in This Lab

| Behaviour | Reason | Impact |
|-----------|--------|--------|
| UPN suffix shows `onmicrosoft.com` | `InfoTech.com` is unverified in Azure | Expected — users sign in with onmicrosoft.com UPN |
| Password writeback requires Entra ID P1 | Free tier limitation | SSPR writeback may be limited — tested in Phase 7 |
| Sync every 30 min | Default interval | Force sync with `Start-ADSyncSyncCycle` for immediate changes |

---

<div align="center">
<sub>⚙️ Entra Connect Configuration | InfoTech.com → Entra ID | azure-hybrid-identity-lab</sub>
</div>