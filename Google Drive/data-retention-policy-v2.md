# Vectrix Data Retention Policy — v2

**Version:** 2  
**Owner:** Jordan Okafor (jordan@vectrix.io)  
**Approved by:** Priya Chandran, Marcus Webb  
**Effective:** October 21, 2025  
**Status:** CURRENT  
**Supersedes:** data-retention-policy-v1.md

---

## 1. Purpose

This document defines how long Vectrix retains data collected from customer environments and internal operations. Version 2 introduces tiered retention for analytics data to control ClickHouse Cloud storage costs as event volume scales.

---

## 2. Retention Schedule

| Data Type         | Retention Period              | Storage System               | Notes                                     |
| ----------------- | ----------------------------- | ---------------------------- | ----------------------------------------- |
| Raw API events    | **90 days**                   | ClickHouse Cloud             | Reduced from 180 days (v1) — see §3       |
| Hourly aggregates | **2 years**                   | ClickHouse Cloud             | Long-term trend analysis                  |
| User account data | Duration of account + 30 days | PostgreSQL (Neon)            | Deleted on account closure + 30 day grace |
| Audit logs        | 1 year                        | S3 (encrypted)               | Compliance requirement                    |
| Database backups  | 30 days rolling               | S3 (encrypted, cross-region) | Automated daily snapshots                 |

---

## 3. Rationale for Change

**Why 90 days instead of 180 days for raw events:**

Storage costs in TimescaleDB were growing at 22% month-over-month as of October 2025. Jordan Okafor's infra cost review (October 20, 2025) projected costs reaching $4K+/month by Q1 2026 under the 180-day policy. Analysis of customer query patterns showed that 94% of dashboard queries reference the last 30 days, and 99% reference the last 90 days. No customer had ever queried data older than 90 days.

The tiered model (90-day raw + 2-year hourly aggregates) preserves long-term trend visibility while reducing storage costs by approximately 40%.

**Decision:** Priya Chandran sided with Jordan's proposal. Marcus Webb initially preferred 180-day raw retention for replay capabilities; accepted the change after reviewing the query pattern data.

---

## 4. Custom Retention (Enterprise)

Enterprise customers may request custom retention periods as a contractual add-on. Per-org retention configuration is in development (VEC-47).

Current custom retention arrangements:

- **Meridian Health Systems (pending):** 1-year raw event retention requested in draft SOW. Requires VEC-47 completion before contract execution.

---

## 5. Deletion Process

Raw events older than 90 days are automatically purged via ClickHouse Cloud's TTL expression on the events table:

```sql
TTL toDateTime(timestamp) + INTERVAL 90 DAY DELETE
```

Hourly aggregates use a separate TTL:

```sql
TTL toDateTime(bucket_hour) + INTERVAL 2 YEAR DELETE
```

User data deletion is triggered manually by Nina Kowalski on account closure. Audit logs are set to S3 lifecycle expiry at 365 days.

---

## 6. Customer Communication

- Existing customers notified of retention change via email (October 28, 2025)
- Updated terms of service reflect the 90-day raw retention
- Customers on Enterprise contracts may negotiate custom retention via Tessa Moreno

---

## 7. Review Cadence

This policy is reviewed quarterly by Jordan Okafor and Marcus Webb. Next scheduled review: January 2026.

---

_Approved: October 21, 2025_  
_Last modified: December 10, 2025 (updated storage system from TimescaleDB to ClickHouse)_  
_Next review: January 2026_
