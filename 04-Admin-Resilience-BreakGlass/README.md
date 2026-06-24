# Admin Access Resilience & Break-Glass — Assessment Record (CAR-04)

> Disclaimer: All data shown is fictitious and created in an isolated Microsoft 365
> lab tenant for demonstration purposes only. No real personal, employee, or
> customer data was used.

Module: M1 - Identity Security
Lab: Lab 4 - Admin Resilience & Break-Glass
Date: 2026-06-24
Author: Wael Mohamed
Status: Completed

---

## Context
Break-glass (emergency access) accounts are the last line of defense when normal
admin access fails (MFA outage, Conditional Access misconfiguration, or lockout).
A misconfigured break-glass account can either fail when needed or become a
permanent backdoor. This lab assesses the tenant's break-glass account against
Microsoft best practices: role assignment, Conditional Access exclusions, and monitoring.

## Approach
| Step | Action | Tool |
|------|--------|------|
| 1 | Reviewed break-glass account role assignment | Entra > Users > Assigned roles |
| 2 | Reviewed Conditional Access policies and exclusions | Entra > Conditional Access |
| 3 | Inspected MFA exclusion list | CA Policy > Exclude |
| 4 | Checked break-glass sign-in activity | Entra > Sign-in logs |
| 5 | Assessed alerting / monitoring coverage | Entra > Monitoring |

## Findings
| # | Finding | Severity |
|---|---------|----------|
| 1 | Break-glass account holds standing Global Administrator role (Active, Permanent, Direct) | Good |
| 2 | Only one Conditional Access policy exists ("Require MFA for all users"); zero Microsoft-managed | Low |
| 3 | Personal / daily-use admin account excluded from MFA alongside break-glass account | CRITICAL |
| 4 | Only one break-glass account exists (Microsoft recommends two for redundancy) | Medium |
| 5 | No proactive sign-in alerting configured for break-glass usage | High |

### Critical Finding Detail
The MFA exclusion list contains a personal, daily-use admin account (Wael Mohamed)
alongside the legitimate break-glass account.
- Risk: Excluding a regularly-used account from MFA creates a permanent attack
  surface. If compromised, an attacker bypasses MFA entirely.
- Best practice: Only dedicated break-glass accounts should be excluded from MFA,
  never personal or daily-use accounts.

## Why It Matters
> Break-glass accounts bypass MFA and Conditional Access by design. With no sign-in
> alerting, their use is invisible. An attacker using one would be undetected.
> Monitoring is the ONLY compensating control for these high-privilege accounts.

## Recommendations
1. Remove the personal account from the MFA exclusion list.
2. Create a SECOND dedicated break-glass account for redundancy.
3. Keep dedicated break-glass accounts excluded from MFA, but configure sign-in
   alerts (Entra / Sentinel) to notify admins immediately on use.
4. Store credentials securely (split knowledge, long passphrase).
5. Review break-glass accounts periodically as part of access governance.

## Environment
| Component | Detail |
|-----------|--------|
| Tenant | Microsoft 365 E5 (lab) |
| Identity | Microsoft Entra ID |
| Focus | Break-glass account, Conditional Access, MFA exclusions |
| Tools | Entra Users, Conditional Access, Sign-in logs |

## Evidence
| # | Screenshot | Shows |
|---|-----------|-------|
| 1 | ./screenshots/01-breakglass-role.png | Standing Global Admin role |
| 2 | ./screenshots/02-ca-policy-mfa.png | MFA policy with exclusions |
| 3 | ./screenshots/03-mfa-exclusion-list.png | Personal account wrongly excluded |
| 4 | ./screenshots/04-breakglass-signins.png | No sign-in activity |

## Next Steps
- Remediate MFA exclusion and add second break-glass account (CAR-05).
- Configure monitoring alerts for emergency account usage.

## Skills Demonstrated
Microsoft Entra ID, Break-Glass Strategy, Conditional Access, MFA Exclusion Review,
Identity Security, Admin Resilience
