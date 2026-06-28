# 🔐 Admin Access Resilience & Break-Glass — Assessment Record (CAR-04)

> ⚠️ **Disclaimer:** All data shown is fictitious and created in an isolated Microsoft 365 lab tenant for demonstration purposes only. No real personal, employee, or customer data was used.

> **Module:** M1 - Identity Security  
> **Lab:** Lab 4 - Admin Resilience & Break-Glass  
> **Date:** 2026-06-24  
> **Author:** Wael Mohamed  
> **Status:** ✅ Completed

---

## 🧭 Context

Break-glass, or emergency access, accounts are the last line of defense when normal admin access fails due to:

- MFA outage
- Conditional Access misconfiguration
- Admin lockout
- PIM activation failure
- Identity provider outage
- Compromised admin accounts

Microsoft guidance recommends creating **two or more emergency access accounts** to reduce the risk of losing administrative access during emergency situations.

Source: Microsoft Learn — Manage emergency access accounts in Microsoft Entra ID  
https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access

A misconfigured break-glass account is a double-edged risk:

1. It can fail when urgently needed.
2. It can silently become a permanent backdoor.

This lab assesses the tenant's break-glass setup across:

- Role assignment
- Conditional Access and MFA exclusions
- Account redundancy
- Sign-in activity
- Monitoring and alerting

---

## 🎯 Objective

Assess whether the tenant has a resilient and secure emergency access model that can recover administrative access during lockout scenarios without creating unnecessary permanent attack paths.

This assessment focuses on:

- Break-glass role assignment
- Conditional Access exclusions
- MFA exclusion scope
- Personal admin account exposure
- Break-glass redundancy
- Sign-in monitoring and alerting
- Identity resilience before broader security and Copilot readiness work

---

## 🧩 Scope

### In Scope

- Microsoft Entra ID break-glass account review
- Global Administrator assignment review
- Conditional Access policy review
- MFA exclusion review
- Sign-in log review
- Monitoring and alerting assessment
- Identity resilience risk documentation

### Out of Scope

- Production tenant assessment
- Real user or customer data
- Full Conditional Access redesign
- Full PIM implementation
- Full Sentinel deployment
- Password vault implementation
- Legal or regulatory compliance opinion

---

## ⚖️ Constraints

### Budget Constraint

The assessment assumes the organization wants to improve admin resilience using existing Microsoft Entra ID and Microsoft 365 capabilities before investing in broader automation, monitoring, or SIEM expansion.

### Deadline Constraint

Break-glass validation is required before expanding Conditional Access policies, admin governance, Copilot readiness remediation, or tenant-wide security changes.

### User Resistance

Administrators may resist removing MFA exclusions from personal or daily-use admin accounts because the exclusion improves convenience.

This creates a security trade-off that must be clearly explained.

### Operational Constraint

Break-glass accounts must remain usable during emergencies while being protected, monitored, and reviewed.

Over-hardening can make the account unusable.

Under-hardening can make the account a backdoor.

---

## 🪪 License Considerations

### Current Lab License

Microsoft 365 E5 lab tenant.

### E3 Fit

Basic review of users, assigned roles, Conditional Access policies, exclusions, and sign-in logs can be started with existing Microsoft Entra and Microsoft 365 admin capabilities, depending on tenant configuration.

### E5 / Add-on Justification

E5, Microsoft Entra ID P2, Microsoft Sentinel, or related advanced capabilities may be justified when the organization requires:

- Advanced identity governance
- PIM for normal admin accounts
- Risk-based Conditional Access
- Advanced sign-in monitoring
- Sentinel analytics rules
- Automated alerting
- Centralized security operations

### Cost Justification

Do not justify E5 only because a break-glass account exists.

Justify advanced licensing or monitoring based on:

- Number of admins
- Criticality of the tenant
- Conditional Access maturity
- Regulatory pressure
- Need for alerting and SIEM integration
- Risk of tenant-wide admin lockout
- Existing security operations maturity

### Trade-off

Keeping emergency accounts excluded from certain controls supports recovery during outages, but increases risk if not monitored.

Adding stronger monitoring improves security but requires operational ownership and response process.

---

## 📋 Approach

| Step | Action | Purpose |
|---|---|---|
| 1 | Reviewed break-glass account assigned roles | Verify standing admin access |
| 2 | Reviewed Conditional Access policies | Check policy coverage |
| 3 | Inspected MFA exclusion list | Identify accounts bypassing MFA |
| 4 | Checked break-glass sign-in logs | Detect usage or misuse |
| 5 | Assessed alerting and monitoring | Validate detection capability |

---

## 🔍 Findings

### 1️⃣ Role Assignment — ✅ Correct

The break-glass account holds Global Administrator as a direct, active, permanent assignment.

```text
Role: Global Administrator
Membership: Direct
State: Active
End time: Permanent
```

### Key Insight

This aligns with the purpose of emergency access: the account must work when normal admin access paths fail.

[Inference] In this lab scenario, standing access is appropriate because dependency on PIM activation could create a recovery failure during an outage or misconfiguration.

---

### 2️⃣ Conditional Access Coverage — ⚠️ Minimal

Only one Conditional Access policy exists:

```text
Policy: Require MFA for all users
State: On
Microsoft-managed policies: 0
```

### Key Insight

A single Conditional Access policy provides limited layered protection.

This is a starting point, not a mature Zero Trust access model.

Recommended future improvements:

- Admin-focused Conditional Access
- Risk-based policies where licensed
- Device compliance requirements where appropriate
- Break-glass exclusions reviewed separately
- Report-only testing before enforcement

---

### 3️⃣ MFA Exclusion List — 🔴 Critical

The "Require MFA for all users" policy excludes two users:

```text
breakglass@...       ✅ legitimate dedicated break-glass account
wamohamed@...        🔴 personal / daily-use admin account
```

### Risk

Excluding a daily-use personal admin account from MFA creates a permanent MFA-bypass attack path.

If the personal account is phished or compromised, the attacker may bypass MFA entirely for that policy scope.

### Key Insight

Excluding a dedicated break-glass account from specific access controls can be intentional for emergency recovery.

Excluding a personal daily-use admin account is a dangerous backdoor disguised as convenience.

---

### 4️⃣ Account Redundancy — 🟠 Gap

Only one break-glass account exists.

Microsoft recommends creating **two or more emergency access accounts** to reduce the impact of losing emergency administrative access.

Source: Microsoft Learn — Manage emergency access accounts in Microsoft Entra ID  
https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access

### Key Insight

A single break-glass account is a single point of failure.

Possible failure causes:

- Account accidentally deleted
- Account disabled
- Password unavailable
- Credential corrupted
- Account compromised
- Account not tested

---

### 5️⃣ Monitoring & Alerting — 🟠 Missing

Break-glass sign-in logs for the last month showed:

```text
No sign-ins found
```

This is good because the account was unused.

However, no proactive sign-in alerting was configured.

### Key Insight

Break-glass accounts are intentionally exceptional.

Monitoring is the compensating control.

[Inference] If an emergency access account signs in and no alert exists, the organization may not know whether the sign-in was authorized emergency use or account compromise.

---

## ❗ Why It Matters

> A break-glass account is a deliberate exception to normal security controls.  
> That makes it powerful and dangerous.

This tenant has two serious operational concerns:

1. A personal daily-use admin account is excluded from MFA.
2. Break-glass usage has no proactive alerting.

A safe emergency access design must balance:

- Availability during lockout
- Least privilege where possible
- Strong credential protection
- Restricted use
- Monitoring and alerting
- Periodic testing

---

## 🧠 Consultant Thinking

This assessment demonstrates that admin resilience is not only about having a break-glass account.

A real break-glass strategy must answer:

- Will the account work during an outage?
- Is there more than one emergency account?
- Is the account independent enough for recovery?
- Are only dedicated accounts excluded from Conditional Access and MFA policies?
- Are personal admin accounts still protected?
- Is every emergency account sign-in monitored?
- Is there a documented procedure for emergency usage?
- Who reviews usage after the emergency?

The biggest consulting risk is treating break-glass as a checkbox.

A break-glass account that is not monitored becomes a high-risk backdoor.

A break-glass account that is over-controlled may fail during the crisis it was created for.

---

## 🧱 Environment

| Component | Detail |
|---|---|
| Tenant | Microsoft 365 E5 lab |
| Identity | Microsoft Entra ID |
| Focus | Break-glass, Conditional Access, MFA exclusions, monitoring |
| Tools | Entra Users, Assigned Roles, Conditional Access, Sign-in logs |
| Scenario type | Admin resilience / emergency access / identity security |

---

## 📸 Evidence

| # | Screenshot | Shows |
|---|---|---|
| 1 | `./screenshots/01-breakglass-global-admin.jpeg` | Standing Global Admin assignment |
| 2 | `./screenshots/02-ca-policy-mfa.jpeg` | Single MFA policy enabled |
| 3 | `./screenshots/03-breakglass-no-signins.jpeg` | No sign-in activity / no alerting evidence |

---

## 👥 Explain Like

### CISO

Emergency access prevents a business-stopping admin lockout, but it also creates high-risk privileged accounts.

The current setup has a valid break-glass account, but a personal admin MFA exclusion and missing alerting create unnecessary risk.

### IT Admin

The break-glass account should be dedicated, protected, tested, and monitored.

Personal admin accounts should not be excluded from MFA for convenience.

Add a second break-glass account and configure sign-in alerts.

### Business User

This does not affect normal business users directly.

It protects the organization by ensuring administrators can recover access during emergencies without leaving unsafe backdoors open.

---

## 🚨 Failure Scenario

### What Can Break?

A personal admin account excluded from MFA is compromised, allowing an attacker to access the tenant without completing MFA.

### Symptoms

- Suspicious admin sign-in
- No MFA challenge for personal admin account
- Unauthorized policy or user changes
- Delayed detection if no alerting exists
- Security team discovers the exclusion after compromise

### Root Cause

- Personal account excluded from MFA
- MFA exclusion list not reviewed
- No break-glass governance process
- No sign-in alerting for emergency accounts
- Convenience override treated as emergency access

### Fix

1. Remove the personal admin account from the MFA exclusion list immediately.
2. Keep only dedicated emergency accounts in emergency exclusions.
3. Create a second dedicated break-glass account.
4. Configure sign-in alerts for all emergency accounts.
5. Review sign-in logs and role assignment changes.
6. Document emergency usage procedure.
7. Review exclusions periodically.

### Prevention

- Maintain two or more emergency access accounts.
- Exclude only dedicated emergency accounts where required.
- Do not exclude daily-use admin accounts from MFA.
- Monitor all emergency account sign-ins.
- Test accounts periodically.
- Store credentials securely using dual-control procedures.
- Review all Conditional Access exclusions before enforcement.

---

## ⚠️ Key Risks Identified

| Risk | Severity | Impact | Recommended Action |
|---|---|---|---|
| Personal account excluded from MFA | 🔴 Critical | Permanent MFA-bypass backdoor | Remove personal account from exclusion immediately |
| No sign-in alerting | 🟠 High | Emergency-account misuse invisible | Configure Entra or Sentinel alerts |
| Single break-glass account | 🟠 Medium | Single point of failure | Create second emergency account |
| Minimal Conditional Access coverage | 🟡 Low | Limited layered protection | Build Zero Trust CA roadmap |
| No documented usage process | 🟠 Medium | Emergency access may be misused or mishandled | Document procedure and review process |

---

## 🛠️ Recommendations

1. 🔴 Remove the personal account from the MFA exclusion list immediately.
2. 🟠 Create a second dedicated break-glass account.
3. 🟢 Configure sign-in alerts for all emergency access accounts.
4. 🔑 Store break-glass credentials securely using dual-control or split-knowledge process.
5. 🔁 Test emergency access periodically.
6. 📋 Document emergency usage procedure.
7. 🧾 Require post-incident review after every break-glass sign-in.
8. 🧠 Keep daily-use admin accounts protected by MFA and Conditional Access.
9. 🧭 Expand Conditional Access roadmap using report-only mode before enforcement.
10. 📊 Consider Sentinel or Log Analytics alerting for emergency sign-ins if operationally justified.

---

## 📌 Business Value

This assessment provides business value by:

- Reducing tenant lockout risk
- Removing a dangerous MFA bypass from a personal admin account
- Improving admin resilience
- Reducing privileged access risk
- Supporting safer Conditional Access expansion
- Creating a stronger identity foundation before Copilot readiness work
- Improving executive confidence in emergency recovery

---

## 🧪 Validation Performed

The assessment validated that:

- A dedicated break-glass account exists.
- The break-glass account has standing Global Administrator access.
- Only one Conditional Access policy exists.
- MFA exclusions include both a legitimate break-glass account and a personal admin account.
- Only one break-glass account exists.
- No recent break-glass sign-ins were found.
- Proactive sign-in alerting is missing.

---

## 💡 Lessons Learned

- Break-glass needs standing, permanent admin access for emergency recovery.
- MFA exclusions must be limited to dedicated emergency accounts only.
- A control that looks convenient can hide a serious risk.
- Personal admin accounts should not be treated as emergency access accounts.
- Monitoring is the compensating control for accounts that bypass normal controls.
- Two or more emergency accounts reduce single point of failure risk.
- Emergency access must be documented, tested, and reviewed.

---

## 🎤 Interview Talking Points

### Question 1

**Why does Microsoft recommend more than one emergency access account?**

**Model Thinking:**  
To reduce the risk of losing all administrative access if one emergency account is unavailable, corrupted, deleted, disabled, or compromised.

Microsoft recommends creating two or more emergency access accounts.

Source: Microsoft Learn — Manage emergency access accounts in Microsoft Entra ID  
https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access

---

### Question 2

**Should a break-glass account be protected by the same MFA and Conditional Access policies as normal admins?**

**Model Thinking:**  
Not necessarily. Emergency accounts exist to recover access when normal controls fail.

However, exceptions must be limited to dedicated emergency accounts and must be heavily monitored.

---

### Question 3

**Why is excluding a personal admin account from MFA dangerous?**

**Model Thinking:**  
Because it creates a permanent MFA bypass for a daily-use account.

If that account is phished or compromised, the attacker may bypass MFA under the policy scope.

---

### Question 4

**What is the compensating control for emergency accounts excluded from MFA or Conditional Access?**

**Model Thinking:**  
Strong monitoring and alerting.

All emergency account sign-ins should be treated as high-priority events requiring investigation and documentation.

---

### Question 5

**How does break-glass relate to Copilot readiness?**

**Model Thinking:**  
Copilot readiness work often involves security, Purview, Conditional Access, and permissions changes.

If a security policy misconfiguration locks out admins, break-glass accounts provide a recovery path.

Identity resilience is part of safe rollout governance.

---

## ➡️ Next Steps

- [x] Identify baseline oversharing patterns *(CAR-01)*
- [x] Review effective access and file-level exposure *(CAR-02)*
- [x] Identify Purview visibility and role dependency gaps *(CAR-03)*
- [x] Assess admin resilience and break-glass setup *(CAR-04)*
- [ ] Remediate MFA exclusion and remove personal admin bypass *(CAR-05)*
- [ ] Create a second dedicated break-glass account
- [ ] Configure Entra or Sentinel sign-in alerts for emergency accounts
- [ ] Document break-glass usage and post-incident review process
- [ ] Test emergency access periodically

---

## 🎯 Skills Demonstrated

`Microsoft Entra ID` · `Break-Glass Strategy` · `Conditional Access` · `MFA Exclusion Review` · `Identity Security` · `Admin Resilience` · `Privileged Access Monitoring` · `Emergency Access Governance` · `Zero Trust` · `Security Consulting Documentation`
