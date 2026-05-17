# 🔑 Self-Service Password Reset (SSPR) — Configuration Reference

> SSPR configuration for the `azure-hybrid-identity-lab` project.
> Allows users to reset their own cloud passwords without IT intervention.

---

## ⚙️ SSPR Settings

| Setting | Value |
|---------|-------|
| **SSPR Enabled** | Selected (scoped to InfoTech users group) |
| **Methods Required** | 1 |
| **Registration on Sign-in** | Yes |
| **Re-confirm after** | 180 days |
| **Notifications** | Notify users on password reset: Yes |

---

## 🔐 Authentication Methods Enabled

| Method | Enabled |
|--------|---------|
| Mobile app notification | ✅ |
| Mobile app code | ✅ |
| Email | ✅ |
| Mobile phone | ✅ |
| Security questions | ❌ |

---

## 👤 User Registration Status

| User | SSPR Registered | Method |
|------|----------------|--------|
| Paula Doe | ✅ Yes | Email |
| Dave Doe | ⏳ Pending | — |
| Sue | ⏳ Pending | — |
| Ram Doe | ⏳ Pending | — |

---

## 🧪 SSPR Test Result

| Field | Value |
|-------|-------|
| **Test User** | `paula@sagaarpkrgmail129.onmicrosoft.com` |
| **SSPR Portal** | `https://passwordreset.microsoftonline.com` |
| **Verification Method** | Email code |
| **Result** | ✅ Password reset successful |
| **Sign-in after reset** | ✅ Confirmed working |

---

## ⚠️ Password Writeback Limitation

| Item | Status |
|------|--------|
| Cloud password updated by SSPR | ✅ Immediately |
| On-premises AD password updated | ❌ Requires Entra ID P1 |
| Writeback configured in Entra Connect | ✅ Enabled (Phase 3) |
| Writeback functional | ❌ P1 licence not available |

Password writeback was enabled during Entra Connect installation in Phase 3.
Once a P1 licence is available, writeback will function automatically
without any additional configuration — the groundwork is already in place.

---

## 🔗 Key URLs

| Purpose | URL |
|---------|-----|
| SSPR Registration | `https://aka.ms/ssprsetup` |
| SSPR Reset Portal | `https://passwordreset.microsoftonline.com` |
| My Apps Portal | `https://myapps.microsoft.com` |
| Admin SSPR Config | `portal.azure.com → Entra ID → Password reset` |

---

<div align="center">
<sub>🔑 SSPR Configuration Reference | InfoTech.com | azure-hybrid-identity-lab</sub>
</div>