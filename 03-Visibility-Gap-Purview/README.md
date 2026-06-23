# 🏛️ Visibility Gap & Role Dependency — Assessment Record (CAR-03)

> ⚠️ **Disclaimer:** All data shown is fictitious and created in an isolated
> Microsoft 365 lab tenant for demonstration purposes only. No real personal,
> employee, or customer data is used.

> **Module:** M1 - Data Security | **Lab:** Lab 3 - Purview Visibility & Classification
> **Date:** 2026-06-23 | **Author:** Wael Mohamed
> **Status:** Completed ✅

---

## 🎯 Context

After reproducing Microsoft 365 oversharing risks in CAR-01 and mapping effective access
in CAR-02, this lab focuses on a different but equally critical question:

**Can the organization actually see where sensitive data exists?**

Microsoft 365 Copilot readiness is not only about permissions and sharing links. It also
depends on whether governance tools such as Microsoft Purview can discover, classify, and
surface sensitive content across SharePoint, OneDrive, Teams, Exchange, and Copilot-related
locations.

This lab investigates visibility gaps in Microsoft Purview and shows how classification and
role assignments affect what administrators can detect.

---

## 🧩 Approach

| Step | Action | Purpose |
|------|--------|---------|
| 1 | Opened Microsoft Purview Content Explorer | Check tenant-wide visibility |
| 2 | Observed no visible data in Content Explorer | Identify visibility gap |
| 3 | Applied a Sensitivity Label manually to `Salaries2026.xlsx` | Create classification signal |
| 4 | Verified label activity in Activity Explorer | Confirm label event was recorded |
| 5 | Attempted to access Content Explorer details | Validate data visibility |
| 6 | Encountered missing role error | Identify role dependency |
| 7 | Added required Content Explorer roles | Enable visibility |
| 8 | Reopened Content Explorer | Validate sensitive data discovery |
| 9 | Reviewed detected sensitive information types | Analyze detection quality |

---

## 🔍 Findings

### 1️⃣ Initial Visibility Gap

At the beginning of the assessment, Content Explorer showed:

> `No data available`

This happened despite the tenant containing a confidential file:

> `Salaries2026.xlsx`

The file contained fictitious salary data, employee names, departments, and ID-like numbers.

### Key Insight

Sensitive data can exist in the tenant while governance visibility remains limited or unavailable.

---

### 2️⃣ Classification Dependency

A Sensitivity Label was manually applied to the file:

> `Confidential`

Activity Explorer later confirmed label activity for the file:

| Field | Value |
|-------|-------|
| Activity | Label changed |
| File | `Salaries2026.xlsx` |
| How applied | Manual |
| Sensitivity label | Confidential / All Employees |
| Old sensitivity label | General / All Employees (unrestricted) |

### Key Insight

Applying labels creates an important classification signal, but labeling alone does not always
produce immediate visibility in Content Explorer.

---

### 3️⃣ Activity Visibility vs Data Visibility

Activity Explorer successfully showed that a label was applied.

However, Content Explorer initially remained inaccessible or empty.

This revealed an important distinction:

| Tool | What It Shows | Status |
|------|---------------|--------|
| **Activity Explorer** | User and label activity | ✅ Visible |
| **Content Explorer** | Where sensitive data exists | ❌ Initially blocked |

### Key Insight

Having visibility into activity does not guarantee visibility into data.

---

### 4️⃣ Role Dependency Gap

When attempting to inspect Content Explorer details, Microsoft Purview displayed a permission error requiring:

- Content Explorer List Viewer
- Content Explorer Content Viewer

This showed that Global Administrator and Compliance Administrator permissions alone were not enough
to access all Content Explorer data.

### Key Insight

Full visibility in Purview requires the correct combination of role assignments, not only broad
administrative privileges.

---

### 5️⃣ Visibility Restored After Role Assignment

After assigning the required Content Explorer roles, Content Explorer became accessible and started
showing sensitive information discovery across the tenant.

Content Explorer showed sensitive information distributed across multiple Microsoft 365 locations:

| Location | Files |
|----------|-------|
| Exchange | 0 |
| OneDrive | 1 |
| SharePoint | 25 |
| Teams | 66 |
| Copilot | 61 |

### Key Insight

Once roles and classification signals were in place, Microsoft Purview was able to surface sensitive
data visibility across the organization.

---

### 6️⃣ Sensitive Information Types Detected

Content Explorer displayed several detected sensitive information types, including:

- All Full Names
- All Medical Terms And Conditions
- Diseases
- Ecuador Unique Identification Number
- UAE Passport Number
- Philippines Passport Number
- Indonesia Passport Number
- EU Passport Number
- Poland Passport
- Lab Test Terms
- Drug Enforcement Agency (DEA) Number
- EU National Identification Number
- Hong Kong Identity Card (HKID) Number

For the file `Salaries2026.xlsx`, Purview detected sensitive information such as:

- Thai Population Identification Code
- EU Tax Identification Number (TIN)
- Japanese My Number Corporate
- All Full Names

Detection confidence varied across high, medium, and low confidence levels.

### Key Insight

Purview can detect sensitive-looking patterns automatically, but detected sensitive info types may
require analyst review and tuning to reduce false positives.

[Inference] Because the lab file contained fictitious ID-like numeric patterns, Microsoft Purview
matched those patterns against multiple built-in sensitive information types from different countries.
This indicates the importance of reviewing confidence levels and detection context before making
governance decisions.

---

## 💥 Why It Matters

> You cannot secure what you cannot see.
>
> You also cannot reliably see sensitive data without classification,
> indexing, and the correct Microsoft Purview roles.

Copilot readiness requires more than checking licenses or assigning Copilot to users.

Organizations must validate:

- Where sensitive data exists
- Whether data is classified
- Whether Purview can detect it
- Whether administrators have the right roles to investigate it
- Whether detections are accurate enough to support remediation decisions

---

## 🚨 Key Risks Identified

| Risk | Explanation |
|------|-------------|
| **Visibility Gap** | Sensitive data existed but was not initially visible in Content Explorer |
| **Role Dependency** | Missing Content Explorer roles blocked access to data insights |
| **Delayed Visibility** | Labeling does not always result in instant Content Explorer visibility |
| **Partial Governance View** | Activity was visible, but data location visibility was initially restricted |
| **Detection Noise** | Built-in sensitive info types may match sample or generic numeric patterns |

---

## ✅ Lessons Learned

- Labels are not just for protection; labels are also a foundation for visibility.
- Activity visibility and content visibility are separate capabilities in Purview.
- Correct role assignment is required before performing a tenant-wide data assessment.
- Content Explorer can reveal sensitive information across SharePoint, Teams, OneDrive, Exchange, and Copilot locations.
- Sensitive information detection must be reviewed carefully to separate true findings from pattern-based false positives.
- Copilot readiness requires classification, visibility, permissions review, and remediation planning.

---

## 🛠️ Environment

| Component | Detail |
|-----------|--------|
| Tenant | Microsoft 365 E5 lab tenant |
| Workloads | SharePoint Online, OneDrive, Teams, Exchange, Copilot |
| Tools | Microsoft Purview Content Explorer, Activity Explorer, Sensitivity Labels |
| Target file | `Salaries2026.xlsx` |
| Data type | Fictitious salaries, names, departments, and ID-like numbers |

---

## 📸 Evidence

### 1️⃣ Content Explorer — Initial No Data Available

![Initial No Data Available](./screenshots/01-content-explorer-no-data.png)

### 2️⃣ Sensitivity Label Applied to Salaries2026.xlsx

![Sensitivity Label Applied](./screenshots/02-label-applied-confidential.png)

### 3️⃣ Activity Explorer — Label Changed Event

![Activity Explorer](./screenshots/03-activity-explorer-label-changed.png)

### 4️⃣ Content Explorer — Permission Required

![Permission Required](./screenshots/04-content-explorer-permission-required.png)

### 5️⃣ Content Explorer — Tenant-Wide Sensitive Data Visibility

![Tenant-Wide Sensitive Data Visibility](./screenshots/05-content-explorer-tenant-visibility.png)

### 6️⃣ Content Explorer — Salaries2026.xlsx Sensitive Info Detection

![Salaries2026.xlsx Sensitive Info Detection](./screenshots/06-salaries-sensitive-info-detection.png)

---

## 🚀 Next Steps

- Validate sensitivity label publishing and label policy scope.
- Create a controlled label taxonomy for Public, Internal, Confidential, and Highly Confidential data.
- Use Microsoft Purview DLP to test enforcement against sensitive data exposure.
- Tune sensitive information detection to reduce false positives.
- Combine Content Explorer findings with SharePoint sharing and effective access review.
- Build a 30/60/90 remediation roadmap for Copilot readiness.

---

## 🧠 Skills Demonstrated

`Microsoft Purview` · `Content Explorer` · `Activity Explorer` · `Sensitivity Labels` ·
`Data Classification` · `Role-Based Access Control` · `Copilot Readiness` ·
`Sensitive Information Detection` · `Data Governance`
