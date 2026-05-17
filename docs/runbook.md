# 📖 Azure Hybrid Identity Lab — Operational Runbook

> Day-to-day operational reference for managing the hybrid identity
> environment across `InfoTech.com` on-premises AD and Microsoft Entra ID.

---

## 📑 Table of Contents

| # | Section |
|---|---------|
| 1 | [Environment Reference](#environment-reference) |
| 2 | [Daily Health Checks](#daily-health-checks) |
| 3 | [Sync Management](#sync-management) |
| 4 | [User Management](#user-management) |
| 5 | [MFA & SSPR](#mfa--sspr) |
| 6 | [Troubleshooting](#troubleshooting) |
| 7 | [Quick Reference](#quick-reference) |

---

## Environment Reference

| Component | Details |
|-----------|---------|
| **Primary DC** | `VM-WINSERV-01` — `192.168.1.10` |
| **Secondary DC** | `VM-WINSERV-02` — `192.168.1.12` |
| **Domain** | `InfoTech.com` |
| **Entra Tenant** | `yourtenant.onmicrosoft.com` |
| **Azure Portal** | `https://portal.azure.com` |
| **My Apps Portal** | `https://myapps.microsoft.com` |
| **Entra Connect** | Installed on `VM-WINSERV-01` |
| **Sync Service** | `ADSync` (Windows Service) |
| **Sync OU** | `OU=Azure_Sync,DC=InfoTech,DC=com` |
| **MFA** | Security Defaults — all users |

---

## Daily Health Checks

**On VM-WINSERV-01:**
```powershell
Import-Module "C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1"

# AD replication
repadmin /replsummary

# Sync service
Get-Service -Name "ADSync"

# Last sync result
Get-ADSyncRunProfileResult | Select-Object -First 1 |
    Select StartDate, RunProfileName, Result

# Sync scheduler
Get-ADSyncScheduler | Select SyncCycleEnabled, NextSyncCyclePolicyType
```

**In Azure Portal:**
```
Entra ID → Users → confirm 4 users show "On-premises sync: Yes"
Entra ID → Password reset → confirm SSPR is enabled
```

---

## Sync Management

### Force a Sync

```powershell
Import-Module "C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1"

# Delta sync — use after small AD changes
Start-ADSyncSyncCycle -PolicyType Delta

# Full sync — use after major changes or new users added
Start-ADSyncSyncCycle -PolicyType Initial
```

### Add a New User to Sync

```powershell
# 1 — Create the user in AD
New-ADUser -Name "Jane Smith" -SamAccountName "jsmith" `
    -UserPrincipalName "jsmith@InfoTech.com" `
    -AccountPassword (ConvertTo-SecureString "TempPass@2026!" -AsPlainText -Force) `
    -Enabled $true -ChangePasswordAtLogon $false `
    -Department "IT"

# 2 — Move user into the sync OU
Move-ADObject -Identity (Get-ADUser "jsmith").DistinguishedName `
    -TargetPath "OU=Azure_Sync,DC=InfoTech,DC=com"

# 3 — Force sync
Start-ADSyncSyncCycle -PolicyType Delta
```

> ⚠️ Users must be in `Azure_Sync` OU to sync to Entra ID.

### Force Password Hash Sync

```powershell
$allUsers = Get-ADSyncCSObject -ConnectorName "InfoTech.com"
foreach ($user in $allUsers) {
    try { Invoke-ADSyncCSObjectPasswordHashSync -CsObject $user }
    catch {}
}
Start-ADSyncSyncCycle -PolicyType Delta
```

---

## User Management

### Reset On-Premises Password

```powershell
Set-ADAccountPassword -Identity "username" `
    -NewPassword (ConvertTo-SecureString "NewPass@2026!" -AsPlainText -Force) `
    -Reset
Set-ADUser -Identity "username" -ChangePasswordAtLogon $false

# Force password hash sync immediately after
$user = Get-ADSyncCSObject -ConnectorName "InfoTech.com" |
    Where-Object { $_.DN -like "*username*" }
Invoke-ADSyncCSObjectPasswordHashSync -CsObject $user
Start-ADSyncSyncCycle -PolicyType Delta
```

### Unlock Account

```powershell
# On-premises AD
Unlock-ADAccount -Identity "username"

# Cloud — Azure Portal: Entra ID → Users → select user → Reset password
```

### Offboard a User

```powershell
# 1 — Disable in AD
Disable-ADAccount -Identity "username"

# 2 — Move out of sync OU
Move-ADObject -Identity (Get-ADUser "username").DistinguishedName `
    -TargetPath "OU=Disabled_Users,DC=InfoTech,DC=com"

# 3 — Sync — user will be disabled in Entra ID automatically
Start-ADSyncSyncCycle -PolicyType Delta
```

---

## MFA & SSPR

### Check MFA Registration Status
```
portal.azure.com → Entra ID → Security
→ Authentication Methods → User Registration Details
```

### Force MFA Re-registration
```
portal.azure.com → Entra ID → Users → select user
→ Authentication methods → Require re-register MFA
```

### User SSPR Links

| Action | URL |
|--------|-----|
| Register SSPR method | `https://aka.ms/ssprsetup` |
| Reset password | `https://passwordreset.microsoftonline.com` |
| Sign into apps | `https://myapps.microsoft.com` |

---

## Troubleshooting

| Symptom | First Check | Fix |
|---------|-------------|-----|
| User not in Entra ID | Is user in `Azure_Sync` OU? | Move user to OU, run delta sync |
| Cloud password wrong | Password hash not synced | Run `Invoke-ADSyncCSObjectPasswordHashSync` |
| Sync service stopped | `Get-Service ADSync` | `Start-Service ADSync` |
| ADSync busy error | Previous sync running | Wait 30s → `Stop-ADSyncSyncCycle` → retry |
| `Import-Module ADSync` fails | Module not in PS path | Use full path as shown above |
| User locked out of cloud | MFA or password issue | Reset in AD, force hash sync |
| SSPR not working | User not registered | Direct to `aka.ms/ssprsetup` |
| AD replication error 8524 | DNS ordering on secondary DC | Fix DNS, disable IPv6, `repadmin /syncall /AdeP` |

---

## Quick Reference

```powershell
# Sync
Import-Module "C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1"
Start-ADSyncSyncCycle -PolicyType Delta
Start-ADSyncSyncCycle -PolicyType Initial
Get-Service -Name "ADSync"
Get-ADSyncScheduler
Get-ADSyncRunProfileResult | Select-Object -First 3

# AD
repadmin /replsummary
Unlock-ADAccount -Identity "username"
Get-ADUser -Filter {LockedOut -eq $true}
```

```
# Azure Portal Navigation
Users list:    Entra ID → Users → All Users
Sync status:   Entra ID → Microsoft Entra Connect → Connect sync
MFA status:    Entra ID → Security → Authentication Methods
SSPR config:   Entra ID → Password reset → Properties
Sign-in logs:  Entra ID → Sign-in logs
```

---

<div align="center">
<sub>📖 Operational Runbook | InfoTech.com Hybrid Identity | azure-hybrid-identity-lab</sub>
</div>