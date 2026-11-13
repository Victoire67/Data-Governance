# Data Access Decision Simulator

**Lab solution: EduConnect Ghana · Data Governance and Integrity**

| | |
|---|---|
| **Role** | DevOps Engineer, EduConnect Ghana |
| **Prepared by** | Victoire Ansima |
| **Date** | 10 November 2026 |

---

## Table of Contents

1. [Policy Reference](#1-policy-reference)
2. [Decision Summary](#2-decision-summary)
3. [Request 1: Marketing Campaign](#3-request-1-marketing-campaign)
4. [Request 2: Analytics Partnership](#4-request-2-analytics-partnership)
5. [Request 3: Archive & Deletion](#5-request-3-archive--deletion)

---

## 1. Policy Reference

Decisions are based on the company's data classification policy, the data lifecycle, and the **Ghana Data Protection Act, 2012 (Act 843)**.

### Data Classification Tiers

| Tier | Examples | Handling rule |
|---|---|---|
| **PUBLIC** | Marketing materials, public course catalogs | Can be shared freely. |
| **INTERNAL** | Aggregated analytics, internal reports | Employees with a business need; not shared externally without approval. |
| **CONFIDENTIAL** | Student PII, grades, payment information | Need-to-know only; Data Owner + DPO approval; encryption, logging and legal agreements before any sharing. |

### Data Lifecycle

```
Create → Store → Use → Share → Archive → Destroy
```

### Least Privilege

Each person or system gets only the minimum data and access needed for the task, for the shortest time needed.

---

## 2. Decision Summary

| Request | Decision | Lifecycle stage |
|---|---|---|
| 1. Marketing Campaign (Sarah Owusu) | ⚠️ Conditional | Use |
| 2. Analytics Partnership (David Mensah) | ❌ Deny | Share |
| 3. Archive & Deletion (James Boateng) | ✅ Approve | Destroy (+ Archive) |

---

## 3. Request 1: Marketing Campaign

### Request Details

| Field | Value |
|---|---|
| **Ticket** | DAR-001 |
| **Requester** | Sarah Owusu, Marketing Manager |
| **Request** | Full student database (names, emails, phone numbers, course enrollments) for a referral campaign starting Friday. |
| **Data classification** | Emails & phone numbers: CONFIDENTIAL · Names & enrollments: INTERNAL |
| **Requester's access level** | INTERNAL only |

### Data Access Decision Form

**Decision:** ⚠️ CONDITIONALLY APPROVED (full database request DENIED)

**Lifecycle stage:** USE (secondary data use), with SHARE if data leaves the student records system.

**Justification**

- **Classification & escalation:** Request includes CONFIDENTIAL fields; requester has INTERNAL access only.
- **Least privilege:** Campaign requires communication capability, not raw database exports.
- **Purpose limitation (Ghana DPA):** Marketing reuse requires explicit opt-in consent and unsubscribe options.

**Action Steps**

1. Decline raw export and submit request to Data Owner & DPO for conditional marketing approval.
2. Filter list strictly to opted-in students; push first name and email directly to email platform.
3. Use tokenised links instead of exposing raw enrollment history; include mandatory unsubscribe mechanisms.
4. Grant time-limited access (expires in 30 days), audit access logs, and delete export lists post-campaign.

### Decision Points Answered

| Question | Answer Summary |
|---|---|
| **Decision & Lifecycle** | Conditionally approve minimal consent audience; deny full database export. Lifecycle stage: USE (with SHARE element). |
| **Least Privilege & Safeguards** | Requester stays at INTERNAL level. Safeguards: consent filtering, data minimisation, controlled platform push, time-bound access, and vendor DPA. |
| **Stakeholder Consultations** | Data Owner (Registrar), DPO/Compliance, Legal, and InfoSec. |

---

## 4. Request 2: Analytics Partnership

### Request Details

| Field | Value |
|---|---|
| **Ticket** | DAR-002 |
| **Requester** | David Mensah, Head of Product |
| **Request** | Give DataInsights Inc. (US-based) access to the AWS production database to analyse student learning patterns. Login credentials were attached to the request. |
| **Data classification** | Student activity logs: INTERNAL · Student profiles: CONFIDENTIAL |
| **Compliance note** | EduConnect is subject to the Ghana Data Protection Act, 2012 (Act 843) |

### Data Access Decision Form

**Decision:** ❌ DENIED (credential leak & excessive exposure). Resubmission required.

**Lifecycle stage:** SHARE (disclosure to an external third party, across borders).

**Justification**

- **Security incident:** Insecure sharing of credentials in a support ticket violates access control policy.
- **Least privilege:** Direct production DB access exposes non-required CONFIDENTIAL student profiles.
- **Compliance (Ghana DPA):** Cross-border transfers require legal processing agreements, DPIA, and transparency.

**Action Steps**

1. Immediately revoke compromised credentials, check AWS CloudTrail logs, and log a security incident.
2. Escalate to DPO, Legal, and CISO to complete a Data Protection Impact Assessment (DPIA).
3. If approved, export pseudonymised activity logs only to an isolated sandbox environment using time-limited IAM roles.

### Decision Points Answered

| Question | Answer Summary |
|---|---|
| **Decision & Lifecycle** | Deny requested direct access. Lifecycle stage: SHARE (cross-border third-party disclosure). |
| **Red Flags & Safeguards** | **Red flags:** credential leakage, direct prod access, lack of DPA/DPIA. **Safeguards:** pseudonymisation, isolated sandbox, read-only IAM, KMS encryption. |
| **Required Documentation** | Data Processing Agreement, DPIA, cross-border transfer documentation, incident report, Data Owner + DPO approval forms. |

---

## 5. Request 3: Archive & Deletion

### Request Details

| Field | Value |
|---|---|
| **Ticket** | DAR-003 |
| **Requester** | Comfort Asante, Customer Support Lead (on behalf of student James Boateng) |
| **Request** | Full deletion of James Boateng's account and associated data ("right to be forgotten"). |
| **Data classification** | All student data: CONFIDENTIAL |
| **Current status** | Last course completed 6 months ago; account inactive; no pending payments or obligations |

### Data Access Decision Form

**Decision:** ✅ APPROVED (with legally required retention exceptions)

**Lifecycle stage:** DESTROY (permanent deletion), with ARCHIVE for the minimal records that must legally be kept.

**Justification**

- **Ghana DPA:** Personal data must not be kept longer than necessary once the primary purpose ends.
- **Classification & privilege:** Full erasure of CONFIDENTIAL records executed strictly by Data Engineering.
- **Exceptions:** Financial and transaction records retained in compliance with statutory accounting laws.

**Action Steps**

1. Verify identity, log request, and perform full system data mapping across primary DBs and third parties.
2. Delete personal records, move required tax records to restricted archives, and issue deletion requests to vendors.
3. Provide written confirmation of deleted vs. legally retained data to the student and close ticket.

### Decision Points Answered

| Question | Answer Summary |
|---|---|
| **Decision & Lifecycle** | Approved with legal retention exceptions. Lifecycle stage: DESTROY (with ARCHIVE for statutory records). |
| **Legal Obligations & Retained Data** | Honor right to be forgotten under Ghana DPA. Financial transactions retained for tax compliance; minimal audit log preserved for accountability. |
| **Support Process** | Verify ID → Log request → Escalate to Data Engineering/DPO → Execute mapping and deletion → Notify processors → Confirm to student. |