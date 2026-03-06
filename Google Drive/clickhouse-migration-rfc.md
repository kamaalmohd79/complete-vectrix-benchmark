# RFC: Migrating from TimescaleDB to ClickHouse for Event Storage

**Author:** Marcus Webb (marcus@vectrix.io)  
**Date:** November 12, 2025  
**Status:** APPROVED (November 26, 2025)  
**Reviewers:** Liam Nakamura, Jordan Okafor  
**Decision:** Proceed with ClickHouse Cloud migration. Target: December 15, 2025.

---

## Summary

We are hitting the practical limits of TimescaleDB for high-volume event ingestion. This RFC proposes migrating the analytics event store from self-hosted TimescaleDB (EC2 r5.2xlarge) to **ClickHouse Cloud** (managed). The migration will improve write throughput by 10–20x, reduce operational overhead, and better position us for the retention and query requirements of enterprise customers like Meridian Health Systems.

---

## Background

As of November 2025, Vectrix ingests approximately 22M API events per day across all customers. TimescaleDB's write throughput is degrading above 30M events/day on our current instance. We project hitting this wall by January 2026 at current growth rates. Additionally:

- Storage costs: TimescaleDB storage is growing 22% month-over-month (Jordan's cost review, Oct 20)
- Query latency: Complex anomaly detection queries are taking 2–4 seconds at current data volumes
- Enterprise requirements: Meridian Health Systems requires 1-year custom retention — TimescaleDB's retention config is global, not per-org

---

## Proposed Solution

**Move all API event storage from TimescaleDB to ClickHouse Cloud.**

ClickHouse is a columnar OLAP database optimized for append-only time-series workloads. It compresses event data 5–10x better than TimescaleDB and handles batch inserts at >100M rows/day without dedicated ops attention.

### Migration Plan

**Phase 1 — Staging validation (Week 1):**

- Jordan provisions ClickHouse Cloud cluster in staging (us-east-1)
- Liam rewrites ingestion worker to write to ClickHouse via native HTTP protocol
- Load test: simulate 52M+ events/day in staging
- Validate query equivalence: same results from ClickHouse as TimescaleDB for all dashboard queries

**Phase 2 — Dual-write (Weeks 2–3):**

- Deploy updated ingestion workers to production writing to both TimescaleDB and ClickHouse
- Backfill historical data from TimescaleDB to ClickHouse
- Validate row counts match

**Phase 3 — Cutover (Week 4):**

- Switch dashboard query layer to read from ClickHouse
- Monitor for anomalies for 48 hours
- Keep TimescaleDB running read-only for 30 days post-cutover as fallback

**Phase 4 — Decommission:**

- Decommission TimescaleDB writes after 2-week stability window
- Terminate EC2 instance after 30-day read-only period

---

## Schema Design

```sql
CREATE TABLE api_events (
    org_id         LowCardinality(String),
    endpoint_id    LowCardinality(String),
    method         LowCardinality(String),
    status_code    UInt16,
    latency_ms     UInt32,
    event_metadata LowCardinality(String),  -- coalesce nulls to ''
    timestamp      DateTime
) ENGINE = ReplacingMergeTree()
ORDER BY (org_id, endpoint_id, timestamp)
TTL toDateTime(timestamp) + INTERVAL 90 DAY DELETE;
```

Note: `event_metadata` must use `LowCardinality(String)` — not `Nullable(String)`. Nulls are coalesced to empty string during ingest. This is a schema incompatibility with TimescaleDB's `varchar` column that must be handled in the migration backfill script.

---

## Data Retention

Changing from **180 days flat** (current TimescaleDB policy) to:

- Raw events: **90 days** (TTL on events table)
- Hourly aggregates: **2 years** (separate aggregates table, separate TTL)

This change has been approved by Priya. Jordan proposed it as part of the infra cost review. Marcus's original preference was 180 days for event replay capability; he accepted 90 days after reviewing query pattern data showing 99% of queries fall within 90 days.

---

## Cost Analysis

|                                   | TimescaleDB (EC2)       | ClickHouse Cloud               |
| --------------------------------- | ----------------------- | ------------------------------ |
| Monthly cost (current)            | ~$400/mo (r5.2xlarge)   | ~$800/mo (projected, growing)  |
| Projected cost at 100M events/day | ~$1,200/mo (larger EC2) | ~$1,100/mo (scales with usage) |
| Operational overhead              | High (self-managed)     | Low (managed)                  |

Note: ClickHouse Cloud monthly cost has since grown to $1,100/mo as of February 2026. Jordan is monitoring the $2K/mo threshold.

---

## Risks

| Risk                           | Mitigation                                                                                   |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| Schema incompatibility         | Identified `event_metadata` null issue; handled in backfill script                           |
| Query result divergence        | Dual-write + row count validation before cutover                                             |
| Write throughput regression    | Staging load test at 52M events/day confirmed p99 18ms (vs TimescaleDB 340ms at this volume) |
| Customer impact during cutover | Cutover scheduled for 11pm PT on weekday; Jordan on standby                                  |

---

## Decision

Approved by Priya Chandran and Marcus Webb, November 26, 2025. Migration started November 25.

**Actual outcome:** Migration completed December 10, 2025. One issue encountered: `event_metadata` data type mismatch (see #engineering Slack thread, Dec 2). Fixed Dec 5, backfill completed Dec 8. Cutover Dec 10. TimescaleDB kept read-only through January 10, 2026.

---

_Marcus Webb — November 12, 2025_  
_-M_
