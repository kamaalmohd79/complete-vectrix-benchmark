# Vectrix Data Retention Policy — v1

**Version:** 1  
**Owner:** Jordan Okafor (jordan@vectrix.io)  
**Approved by:** Marcus Webb  
**Effective:** September 15, 2025  
**Status:** SUPERSEDED — see data-retention-policy-v2.md

---

## 1. Purpose

This document defines how long Vectrix retains data collected from customer environments and internal operations.

---

## 2. Retention Schedule

| Data Type         | Retention Period              | Storage System               | Notes                                     |
| ----------------- | ----------------------------- | ---------------------------- | ----------------------------------------- |
| Raw API events    | **180 days**                  | TimescaleDB (EC2)            | All raw event data uniformly retained     |
| User account data | Duration of account + 30 days | PostgreSQL (Neon)            | Deleted on account closure + 30 day grace |
| Audit logs        | 1 year                        | S3 (encrypted)               | Compliance requirement                    |
| Database backups  | 30 days rolling               | S3 (encrypted, cross-region) | Automated daily snapshots                 |

---

## 3. Rationale

**180 days for raw events:** Provides customers a full 6-month lookback window. Supports trend analysis, capacity planning, and incident retrospectives. Storage costs are manageable at current event volume.

---

## 4. Deletion Process

Raw events older than 180 days are automatically purged via TimescaleDB's native retention policy (chunk drop). User data deletion is triggered manually by Nina Kowalski on account closure. Audit logs are set to S3 lifecycle expiry at 365 days.

---

## 5. Customer Data

Vectrix does not retain raw API payloads — only metadata (endpoint, method, status code, latency, org ID, timestamp). No PII is stored in the events table.

---

## 6. Review Cadence

This policy is reviewed quarterly by Jordan Okafor and Marcus Webb.

---

_Approved: September 15, 2025_  
_Next review: December 15, 2025_
