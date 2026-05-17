# 🔐 Conditional Access & MFA Configuration

> Documents the MFA setup and Conditional Access policies designed
> for the `azure-hybrid-identity-lab` project.
>
> **Licence status:** Entra ID P1 not available on free Azure trial
> with personal Microsoft account. Security Defaults active for MFA.
> CA policies are fully designed and documented for production deployment.

---

## 🔐 Current MFA Status

| Setting | Value |
|---------|-------|
| **Active Method** | Security Defaults |
| **Licence** | Entra ID Free |
| **MFA Coverage** | All users — required at sign-in |
| **Admin MFA** | Always enforced |
| **Legacy Auth** | Blocked |
| **Conditional Access** | Not active — P1 required |

---

## 👤 MFA Registration Status

| User | Cloud UPN | MFA Registered | Method |
|------|-----------|---------------|--------|
| Paula Doe | `paula@sagaarpkrgmail129.onmicrosoft.com` | ✅ Yes | Microsoft Authenticator |
| Dave Doe | `dave@sagaarpkrgmail129.onmicrosoft.com` | ⏳ Pending | — |
| Sue | `sue@sagaarpkrgmail129.onmicrosoft.com` | ⏳ Pending | — |
| Ram Doe | `rdoe@sagaarpkrgmail129.onmicrosoft.com` | ⏳ Pending | — |

---

## 📋 Designed Conditional Access Policies

### CA001 — Require MFA for All Cloud Apps

| Field | Value |
|-------|-------|
| **Policy Name** | CA001 - Require MFA for All Cloud Apps |
| **Status** | Designed — pending P1 licence |
| **Users** | All users |
| **Target Resources** | All cloud apps |
| **Conditions** | None — applies to all sign-ins |
| **Grant** | Require MFA |
| **Deploy Mode** | Report-only → On |

---

### CA002 — Block Sign-In Outside Canada

| Field | Value |
|-------|-------|
| **Policy Name** | CA002 - Block Sign-In Outside Canada |
| **Status** | Designed — pending P1 licence |
| **Users** | All users |
| **Target Resources** | All cloud apps |
| **Conditions** | Location: Include Any / Exclude Canada |
| **Named Location** | Canada (country-based) |
| **Grant** | Block access |
| **Deploy Mode** | Report-only first — then On after log validation |

---

### CA003 — Require MFA for Admin Roles

| Field | Value |
|-------|-------|
| **Policy Name** | CA003 - Require MFA for Admin Roles |
| **Status** | Designed — pending P1 licence |
| **Users** | Directory roles: Global Administrator |
| **Target Resources** | All cloud apps |
| **Conditions** | None — always applies |
| **Grant** | Require MFA |
| **Deploy Mode** | On immediately |

---

## 🔧 Production Deployment Sequence

When P1 licence is available:

```
1. Activate Entra ID P1 → assign to all users
2. Disable Security Defaults (conflicts with CA)
3. Create CA001 → Report-only → test sign-in → switch to On
4. Create Named Location: Canada
5. Create CA002 → Report-only → validate logs → switch to On
6. Create CA003 → On immediately (admin scope only)
7. Monitor sign-in logs for 48 hours before full enforcement
```

---

## ⚠️ Licence Requirement Notes

| Attempt | Result |
|---------|--------|
| Azure portal → Licences → Try/Buy | Blocked — personal account limitation |
| entra.microsoft.com → Try/Buy | Blocked — same limitation |
| M365 Developer Program | Not available for this account type |
| **Resolution** | Security Defaults active — CA policies documented for deployment |

---

<div align="center">
<sub>🔐 Conditional Access Reference | InfoTech.com | azure-hybrid-identity-lab</sub>
</div>