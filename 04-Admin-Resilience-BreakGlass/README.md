# 🔐 Admin Access Resilience & Break-Glass — Assessment Record (CAR-04)

> ⚠️ Disclaimer: All data shown is fictitious and created in an isolated Microsoft 365
> lab tenant for demonstration purposes only. No real personal, employee, or customer
> data was used.

Module: M1 - Identity Security
Lab: Lab 4 - Admin Resilience & Break-Glass
Date: 2026-06-24
Author: Wael Mohamed
Status: ✅ Completed

---

## 🧭 Context
Break-glass (emergency access) accounts are the last line of defense when normal admin
access fails — an MFA outage, a Conditional Access misconfiguration, or an admin lockout.
A misconfigured break-glass account is a double-edged risk: it can FAIL when urgently
needed, or silently become a PERMANENT BACKDOOR. This lab assesses the tenant's
break-glass setup against Microsoft best practices across three layers: role assignment,
Conditional Access / MFA exclusions, and monitoring.

---

## 📋 Approach
| Step | Action | Purpose |
|------|--------|---------|
| 1 | Reviewed break-glass account assigned roles | Verify standing admin access |
| 2 | Reviewed Conditional Access policies | Check policy coverage |
| 3 | Inspected MFA exclusion list | Identify who bypasses MFA |
| 4 | Checked break-glass sign-in logs | Detect usage / misuse |
| 5 | Assessed alerting & monitoring | Validate detection capability |

---

## 🔍 Findings

### 1. Role Assignment — ✅ Correct
The break-glass account (breakglass@) holds Global Administrator as a Direct, Active,
Permanent assignment.

`Role: Global Administrator | Membership: Direct | State: Active | End time: Permanent`

Key Insight: This aligns with Microsoft best practice. Break-glass needs STANDING
(not PIM-eligible) access — if it depended on PIM and PIM failed during an outage,
the account would be useless exactly when needed most.

### 2. Conditional Access Coverage — ⚠️ Minimal
Only ONE Conditional Access policy exists: "Require MFA for all users" (State: On).
Microsoft-managed policies: 0.

Key Insight: A single CA policy means limited layered protection. Worth expanding
(risk-based, device compliance) as part of a Zero Trust roadmap.

### 3. MFA Exclusion List — 🔴 CRITICAL
The "Require MFA for all users" policy excludes 2 users:
- breakglass@25rgrc... → ✅ legitimate dedicated break-glass account
- wamohamed@25rgrc... (Wael Mohamed) → 🔴 personal / daily-use admin account

Risk: Excluding a regularly-used personal account from MFA creates a PERMANENT attack
surface. If that account is phished or compromised, the attacker bypasses MFA entirely.

Key Insight: Excluding break-glass from MFA is correct and intentional. Excluding a
personal daily-use account is a dangerous backdoor disguised as convenience.

### 4. Account Redundancy — 🟠 Gap
Only ONE break-glass account exists.

Key Insight: Microsoft recommends TWO break-glass accounts to avoid a single point of
failure (e.g. one account corrupted, expired, or accidentally deleted).

### 5. Monitoring & Alerting — 🟠 Missing
Break-glass sign-in logs (last 1 month): "No sign-ins found" — account unused (good).
However, NO proactive sign-in alerting is configured.

Key Insight: Break-glass accounts bypass MFA and Conditional Access by design.
Monitoring is the ONLY compensating control. Without alerts, emergency-account usage
is completely invisible.

---

## ❗ Why It Matters
> A break-glass account is a deliberate exception to every security control. That makes
> it powerful — and dangerous. Two things turn it from a safety net into a liability:
> (1) excluding the wrong accounts from MFA, and (2) having zero monitoring on its use.
> This tenant has both gaps.

---

## 🛠️ Recommendations
1. 🔴 Remove the personal account (wamohamed@) from the MFA exclusion list immediately.
2. 🟠 Create a SECOND dedicated break-glass account for redundancy.
3. 🟢 Configure sign-in alerts (Microsoft Entra / Sentinel) to notify admins instantly
   when any break-glass account signs in.
4. 🔑 Store break-glass credentials securely (long passphrase, split knowledge).
5. 🔁 Review break-glass accounts periodically as part of access governance.

---

## ⚖️ Key Risks Identified
| Risk | Severity | Impact |
|------|----------|--------|
| Personal account excluded from MFA | 🔴 Critical | Permanent MFA-bypass backdoor |
| No sign-in alerting | 🟠 High | Emergency-account misuse invisible |
| Single break-glass account | 🟠 Medium | Single point of failure |
| Minimal Conditional Access coverage | 🟡 Low | Limited layered protection |

---

## 💡 Lessons Learned
- Break-glass needs standing, permanent admin access — not PIM-eligible.
- MFA exclusions must be limited to dedicated emergency accounts ONLY.
- A control that "looks secure" (MFA exclusion) can hide a real risk (personal account).
- Monitoring is the only safety net for accounts that bypass all other controls.

---

## 🧱 Environment
| Component | Detail |
|-----------|--------|
| Tenant | Microsoft 365 E5 (lab) |
| Identity | Microsoft Entra ID |
| Focus | Break-glass, Conditional Access, MFA exclusions, monitoring |
| Tools | Entra Users, Assigned Roles, Conditional Access, Sign-in logs |

---

## 📸 Evidence
| # | Screenshot | Shows |
|---|-----------|-------|
| 1 | ./screenshots/01-breakglass-global-admin.png | Standing Global Admin (Active, Permanent) |
| 2 | ./screenshots/02-ca-policy-mfa.png | Single MFA policy, On |
| 3 | ./screenshots/03-breakglass-no-signins.png | No sign-in activity / no alerting |

---

## ➡️ Next Steps
- Remediate MFA exclusion + add second break-glass account → CAR-05 (Remediation).
- Configure Entra/Sentinel sign-in alerts for emergency accounts.

---

## 🎯 Skills Demonstrated
Microsoft Entra ID · Break-Glass Strategy · Conditional Access · MFA Exclusion Review ·
Identity Security · Admin Resilience · Privileged Access Monitoring
