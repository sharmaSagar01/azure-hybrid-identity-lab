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

| # | Phase | Status |
|---|-------|--------|
| 1 | Set up Azure free trial + explore Entra ID portal | ⏳ Pending |
| 2 | Prepare on-premises AD for hybrid sync | ⏳ Pending |
| 3 | Install and configure Microsoft Entra Connect | ⏳ Pending |
| 4 | Verify user sync — on-prem AD → Entra ID | ⏳ Pending |
| 5 | Configure Multi-Factor Authentication (MFA) | ⏳ Pending |
| 6 | Configure Conditional Access policies | ⏳ Pending |
| 7 | Configure Self-Service Password Reset (SSPR) | ⏳ Pending |
| 8 | Runbook + final documentation + GitHub push | ⏳ Pending |

---

## 🎯 What You'll Have at the End

| Capability | Details |
|------------|---------|
| **Hybrid Identity** | On-prem AD users synced to Entra ID |
| **Cloud MFA** | Users prompted for MFA when signing into cloud apps |
| **Conditional Access** | Policies enforcing MFA based on location/risk |
| **SSPR** | Users can reset their own password without calling IT |
| **Unified Admin** | Manage identities from both AD and Entra ID portal |

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

<div align="center">
<sub>☁️ Built for learning • ⭐ Star if you find this useful • More phases coming soon</sub>
</div>