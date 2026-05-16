# 🔐 Conditional Access & MFA Configuration

> Documents the MFA setup and Conditional Access policies configured
> in the `azure-hybrid-identity-lab` project.

---

## 🔐 MFA Configuration

| Setting | Value |
|---------|-------|
| **Method** | Security Defaults (free tier) |
| **Scope** | All users in tenant |
| **MFA Registration Period** | 14 days grace for new users |
| **Admins** | Always require MFA |
| **Legacy Auth** | Blocked |

---

## 👤 MFA Registration Status

| User | Cloud UPN | MFA Registered | Method |
|------|-----------|---------------|--------|
| Paula Doe | `paula@sagaarpkrgmail129.onmicrosoft.com` | ✅ Yes | Microsoft Authenticator |
| Dave Doe | `dave@sagaarpkrgmail129.onmicrosoft.com` | ⏳ Pending | — |
| Sue | `sue@sagaarpkrgmail129.onmicrosoft.com` | ⏳ Pending | — |
| Ram Doe | `rdoe@sagaarpkrgmail129.onmicrosoft.com` | ⏳ Pending | — |

---

## ⚙️ Security Defaults — What It Enforces

| Policy | Status |
|--------|--------|
| All users must register MFA | ✅ Enabled |
| Admins require MFA on every sign-in | ✅ Enabled |
| Block legacy authentication protocols | ✅ Enabled |
| MFA required for Azure management | ✅ Enabled |

> Security Defaults is the free-tier equivalent of Conditional Access.
> Full Conditional Access policies require Entra ID P1 licence.

---

## 🧪 MFA Test Result

| Test | Result |
|------|--------|
| Sign-in URL | `https://myapps.microsoft.com` |
| Test User | `paula@sagaarpkrgmail129.onmicrosoft.com` |
| Password accepted | ✅ |
| MFA challenge triggered | ✅ |
| Authenticator approval | ✅ |
| Portal access granted | ✅ |

---

## 📋 Conditional Access Policies

> ⏳ Configured in Phase 6 — populated as policies are created.

| Policy Name | Status | Scope | Condition | Action |
|-------------|--------|-------|-----------|--------|
| Require MFA for all cloud apps | ⏳ Phase 6 | All users | Any location | Require MFA |
| Block sign-in outside Canada | ⏳ Phase 6 | All users | Non-CA location | Block |

---

<div align="center">
<sub>🔐 MFA & Conditional Access Reference | InfoTech.com | azure-hybrid-identity-lab</sub>
</div>