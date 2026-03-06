# Vectrix Data Retention & Privacy Policy

> **Note:** This document lives in Notion (Vectrix workspace). A newer version exists in Google Drive (`data-retention-policy-v2.md`) — but this Notion version contains an additional section on **Customer Data Export** that was not carried over to the Drive version when the document was migrated.

---

**Author:** Jordan Okafor  
**Created:** September 20, 2025  
**Last Updated in Notion:** October 15, 2025  
**Notion Page ID:** 7f3a9b2c-1d4e-4f8a-bc6d-8e2f01a4c9b0  
**Workspace:** Vectrix Engineering

---

## 1. Purpose

This policy defines how Vectrix retains data collected from customer environments and internal systems, and what rights customers have over their data.

---

## 2. Data Types We Collect

Vectrix collects the following categories of data from customer environments:

- **API event metadata**: HTTP method, endpoint path, status code, response latency (ms), timestamp, organization ID
- **No raw payloads**: We do not store request or response bodies
- **No PII**: We do not store user-identifying data from API calls
- **Account data**: Name, email, billing info for users and organizations

---

## 3. Retention Schedule

| Data Type | Retention | Notes |
|---|---|---|
| Raw API events | 180 days | All raw events uniformly retained (as of Sept 2025) |
| User account data | Account lifetime + 30 days | Grace period for account recovery |
| Audit logs | 1 year | S3, encrypted |
| Backups | 30 days rolling | Automated, cross-region S3 |

> ⚠️ **Note (added Oct 15):** This retention schedule is under review. Jordan is proposing moving to 90-day raw + 2-year aggregated model to control storage costs. Decision pending Priya approval. See Notion comment thread.

---

## 4. Deletion Process

Raw events are automatically deleted via TimescaleDB retention policy (chunk drop) after the retention period. No manual intervention required. User data deletion is triggered manually by Nina Kowalski upon account closure request.

---

## 5. Customer Data Export

> ⚠️ **THIS SECTION WAS NOT MIGRATED TO GOOGLE DRIVE** — The Drive version (`data-retention-policy-v2.md`) does not include the export rights section below. This is an oversight from the doc migration in October 2025. Jordan flagged this in a Notion comment but it has not been resolved.

### 5.1 Customer Right to Export

Customers may request a full export of their API event data at any time by contacting support@vectrix.io. Exports will be delivered within 5 business days as a gzipped JSONL file.

**Export format:**
```json
{
  "org_id": "org_abc123",
  "endpoint_id": "ep_xyz789",
  "method": "GET",
  "path": "/api/v1/users",
  "status_code": 200,
  "latency_ms": 142,
  "timestamp": "2025-10-01T14:32:11Z"
}
```

### 5.2 Pre-Deletion Export

When an account is closed, Vectrix will notify the customer 14 days in advance and offer a final data export before deletion. If the customer does not respond within 14 days, data will be deleted per the standard retention schedule.

### 5.3 Portability

Vectrix does not lock customers into proprietary formats. All exports are in open formats (JSONL, CSV). Customers may use exported data with any third-party analytics tool.

---

## 6. Security

- Data in transit: TLS 1.2+
- Data at rest: AES-256 (AWS-managed keys)
- API keys: bcrypt-hashed, never logged
- Auth: Firebase (transitioning to Clerk — see Engineering Notion)

---

## 7. Customer Communication

Changes to this policy will be communicated to customers via email at least 14 days before taking effect. The updated policy will be posted to our website (vectrix.io/privacy).

---

## 8. Contact

Questions about this policy: privacy@vectrix.io  
Data deletion requests: support@vectrix.io

---

*This document is maintained in Notion by Jordan Okafor. For the current Drive version, see `data-retention-policy-v2.md` in the Engineering folder. Note: Section 5 (Customer Data Export) has not been synced to Drive.*
