# ☁️ Azure Hybrid Identity Lab — Microsoft Entra ID + On-Premises AD

> Extends the existing `InfoTech.com` Active Directory environment into the cloud by connecting
> the on-premises domain to **Microsoft Entra ID** (formerly Azure AD) using
> Microsoft Entra Connect — enabling hybrid identity, MFA, Conditional Access, and SSPR
> across both on-premises and cloud resources.

<div align="center">

![Azure](https://img.shields.io/badge/Microsoft_Azure-Free_Trial-0078D4?style=flat-square&logo=microsoftazure)
![Entra ID](https://img.shields.io/badge/Microsoft_Entra_ID-Hybrid_Identity-0078D4?style=flat-square)
![Windows Server](https://img.shields.io/badge/Windows_Server-2025-blue?style=flat-square&logo=windows)
![Active Directory](https://img.shields.io/badge/Domain-InfoTech.com-darkblue?style=flat-square)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat-square)

</div>

---

## 📌 Overview

Hybrid identity is the standard for modern enterprise IT — most organisations run a
mix of on-premises Active Directory and Microsoft 365 / Azure cloud services.
This project bridges the existing `InfoTech.com` on-premises AD environment into
Microsoft Entra ID, creating a unified identity layer that spans both environments.

**What this project demonstrates:**

- Configuring Microsoft Entra Connect to sync on-premises AD users to the cloud
- Implementing cloud-based MFA and Conditional Access policies
- Enabling Self-Service Password Reset (SSPR) for domain users
- Understanding the hybrid identity architecture used in real enterprise environments
- Managing identities across on-premises and cloud from a single control plane

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Microsoft Azure (Cloud)                       │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │              Microsoft Entra ID (Tenant)                 │  │
│   │                                                          │  │
│   │   Synced Users:  paula.doe, dave.doe, sue, ram.doe      │  │
│   │   MFA Policies   Conditional Access   SSPR              │  │
│   └─────────────────────────────────────────────────────────┘  │
└──────────────────────────┬──────────────────────────────────────┘
                           │ Entra Connect Sync
                           │ (HTTPS — port 443)
┌──────────────────────────┴──────────────────────────────────────┐
│                    On-Premises (VMware Lab)                      │
│                                                                 │
│   VM-WINSERV-01 (192.168.1.10) — Primary DC                    │
│   InfoTech.com Active Directory                                 │
│   Microsoft Entra Connect installed here                        │
│                                                                 │
│   VM-WINSERV-02 (192.168.1.12) — Secondary DC                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🖥️ Environment

<table>
<tr>
<td width="50%" valign="top">

**On-Premises**
| Component | Details |
|-----------|---------|
| **Primary DC** | `VM-WINSERV-01` — `192.168.1.10` |
| **Secondary DC** | `VM-WINSERV-02` — `192.168.1.12` |
| **Domain** | `InfoTech.com` |
| **OS** | Windows Server 2025 |
| **Entra Connect** | Installed on VM-WINSERV-01 |

</td>
<td width="50%" valign="top">

**Cloud**
| Component | Details |
|-----------|---------|
| **Platform** | Microsoft Azure (Free Trial) |
| **Identity Service** | Microsoft Entra ID |
| **Sync Tool** | Microsoft Entra Connect |
| **Subscription** | Pay-As-You-Go (Free $200 credit) |
| **Tenant Domain** | `*.onmicrosoft.com` |

</td>
</tr>
</table>

---

## 📁 Repository Structure

```
azure-hybrid-identity-lab/
│
├── config/
│   ├── entra-connect-settings.md       # Entra Connect sync configuration   ⏳
│   ├── conditional-access-policies.md  # Conditional Access policy setup     ⏳
│   └── sspr-config.md                  # Self-Service Password Reset config  ⏳
│
├── screenshots/
│   └── (phase screenshots added as project progresses)
│
├── docs/
│   └── runbook.md                      # Operational runbook                 ⏳
│
└── README.md
```

> ⏳ = In progress — added as the project develops

---

## 🧩 Build Progress

| #   | Phase                                             | Status     |
| --- | ------------------------------------------------- | ---------- |
| 1   | Set up Azure free trial + explore Entra ID portal | ⏳ Pending |
| 2   | Prepare on-premises AD for hybrid sync            | ⏳ Pending |
| 3   | Install and configure Microsoft Entra Connect     | ⏳ Pending |
| 4   | Verify user sync — on-prem AD → Entra ID          | ⏳ Pending |
| 5   | Configure Multi-Factor Authentication (MFA)       | ⏳ Pending |
| 6   | Configure Conditional Access policies             | ⏳ Pending |
| 7   | Configure Self-Service Password Reset (SSPR)      | ⏳ Pending |
| 8   | Runbook + final documentation + GitHub push       | ⏳ Pending |

---

## 🎯 What You'll Have at the End

| Capability             | Details                                               |
| ---------------------- | ----------------------------------------------------- |
| **Hybrid Identity**    | On-prem AD users synced to Entra ID                   |
| **Cloud MFA**          | Users prompted for MFA when signing into cloud apps   |
| **Conditional Access** | Policies enforcing MFA based on location/risk         |
| **SSPR**               | Users can reset their own password without calling IT |
| **Unified Admin**      | Manage identities from both AD and Entra ID portal    |

---

## ⚙️ Prerequisites

Before starting Phase 1 — confirm the following:

**On-Premises Lab:**

```powershell
# Run on VM-WINSERV-01
# Confirm AD is healthy
repadmin /replsummary

# Confirm domain is functional
Get-ADDomain | Select DNSRoot, DomainMode, PDCEmulator
```

**Azure Free Trial:**

- Sign up at `portal.azure.com` using a Microsoft account
- $200 free credit for 30 days — sufficient for this entire project
- No charges if credit is not exceeded
- Required: a valid credit card for identity verification (not charged)

---

---

# ✅ Phase 1 — Azure Free Trial Setup & Entra ID Exploration

## 📋 What This Phase Covers

Setting up the Azure environment, exploring the Microsoft Entra ID portal,
and understanding the key components before connecting anything to the
on-premises AD. Getting familiar with the portal now makes every subsequent
phase significantly easier.

---

## 🚀 Part A — Sign Up for Azure Free Trial

Go to `https://portal.azure.com` and sign in with a Microsoft account.
If you don't have one create a free account at `https://account.microsoft.com`.

**What you get with the free trial:**

| Item          | Details                                               |
| ------------- | ----------------------------------------------------- |
| Free credit   | $200 USD for 30 days                                  |
| Free services | 55+ services free for 12 months                       |
| Credit card   | Required for verification — not charged within credit |
| Entra ID      | Free tier included — sufficient for this project      |

> ⚠️ **Important:** Set a spending limit reminder. Go to
> **Cost Management → Budgets → Add** and create a $10 alert.
> This ensures you get notified well before hitting the $200 limit.

---

## 🗺️ Part B — Explore the Azure Portal Layout

Once logged in, take 10 minutes to familiarise yourself with the layout
before touching anything. This saves significant time in later phases.

```
Azure Portal (portal.azure.com)
│
├── Home
│   ├── Recent resources       ← shortcuts to things you've visited
│   ├── All services           ← full service catalogue
│   └── Dashboard              ← customisable overview
│
├── Microsoft Entra ID         ← THE main service for this project
│   ├── Overview               ← tenant info, user count, directory ID
│   ├── Users                  ← where synced users will appear
│   ├── Groups                 ← cloud groups and synced AD groups
│   ├── Devices                ← hybrid-joined devices (Phase 7)
│   ├── Applications           ← enterprise apps and SSO
│   └── Security
│       ├── Conditional Access ← policy engine (Phase 6)
│       ├── MFA                ← multi-factor auth (Phase 5)
│       └── Password reset     ← SSPR (Phase 7)
│
├── Subscriptions              ← billing and credit usage
└── Cost Management            ← monitor spend against $200 credit
```

---

## 🔍 Part C — Note Your Tenant Details

After signing in, navigate to:

```
Microsoft Entra ID → Overview
```

Note down the following — you will need these in later phases:

| Field              | Where to Find It                                   | Your Value |
| ------------------ | -------------------------------------------------- | ---------- |
| **Tenant ID**      | Entra ID → Overview → Basic Information            | Save this  |
| **Primary Domain** | Entra ID → Overview → e.g. `xxxxx.onmicrosoft.com` | Save this  |
| **Directory Name** | Top right of portal                                | Save this  |

---

## 🔍 Part D — Understand the Entra ID Free Tier

This project uses the **Entra ID Free** tier which is included in the
Azure free trial. Here's what's available and what requires a paid licence:

| Feature                       | Free Tier                   | Required For  |
| ----------------------------- | --------------------------- | ------------- |
| User sync from on-prem AD     | ✅ Included                 | Phase 3       |
| MFA (Microsoft Authenticator) | ✅ Included                 | Phase 5       |
| Self-Service Password Reset   | ✅ Included (cloud only)    | Phase 7       |
| Conditional Access            | ⚠️ Limited — basic policies | Phase 6       |
| Entra ID P1 features          | ❌ Requires licence         | Advanced CA   |
| Entra ID P2 features          | ❌ Requires licence         | Risk-based CA |

> **For this lab:** The free tier is sufficient for all phases.
> If you want to explore Conditional Access more deeply,
> you can activate a free 30-day Entra ID P2 trial within the portal.

---

## ⚙️ Part E — Verify On-Premises AD is Ready

Before any sync work begins, confirm the on-premises environment is healthy.
Run these on **VM-WINSERV-01**:

```powershell
# Check AD replication is clean
repadmin /replsummary

# Check domain functional level — needs to be Windows Server 2008 R2 or higher
Get-ADDomain | Select DNSRoot, DomainMode, PDCEmulator

# Check domain forest level
Get-ADForest | Select Name, ForestMode

# List current AD users that will be synced
Get-ADUser -Filter * -Properties EmailAddress |
    Select DisplayName, SamAccountName, UserPrincipalName, EmailAddress
```

**Expected results:**

| Check                | Expected                                |
| -------------------- | --------------------------------------- |
| Replication failures | 0                                       |
| Domain Mode          | Windows2016Domain or higher             |
| PDC Emulator         | VM-DEV-WINSERV-01                       |
| Users visible        | paula.doe, dave.doe, sue, ram.doe, etc. |

---

## ⚙️ Part F — Check UPN Suffixes on Your AD Users

This is important. When Entra Connect syncs users, it uses their
**User Principal Name (UPN)** as their cloud identity. The issue is that
`InfoTech.com` is a private domain — Azure won't be able to verify it —
so users will sync with the `onmicrosoft.com` suffix instead.

**Check current UPNs:**

```powershell
Get-ADUser -Filter * | Select DisplayName, UserPrincipalName
```

You'll likely see:

```
paula.doe@InfoTech.com
dave.doe@InfoTech.com
```

**This is fine for the lab.** When users sync to Azure, they'll appear as:

```
paula.doe@yourtenant.onmicrosoft.com
```

> If you owned a real public domain (e.g. `infotech.ca`) you could add it
> as a verified custom domain in Entra ID and users would keep their UPN.
> For this lab the `onmicrosoft.com` suffix works perfectly.

---

## ✅ Outcome

- Azure free trial active — $200 credit available ✅
- Azure Portal navigation understood ✅
- Tenant ID and primary domain noted ✅
- Entra ID Free tier confirmed sufficient for all phases ✅
- On-premises AD confirmed healthy — replication clean, users visible ✅
- UPN suffix situation understood — `onmicrosoft.com` will be used ✅
- Spending alert configured in Cost Management ✅

---

## 📸 Screenshots

<p align="center">
  <img src="screenshots/phase1-img1.png" width="45%"
       title="Azure Portal home — free trial active with $200 credit" /
</p>
---
