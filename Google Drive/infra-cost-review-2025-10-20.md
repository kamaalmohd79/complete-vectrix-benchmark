# Infra Cost Review — October 20, 2025

**Date:** October 20, 2025  
**Attendees:** Jordan Okafor, Marcus Webb, Priya Chandran  
**Purpose:** Review AWS storage cost growth and propose action  
**Notes:** Jordan Okafor

---

## Current Costs (September 2025)

| Service                          | Monthly Cost   | Notes                         |
| -------------------------------- | -------------- | ----------------------------- |
| AWS EC2 (TimescaleDB r5.2xlarge) | $396/mo        | Analytics DB                  |
| AWS EKS (compute)                | ~$1,100/mo     | Backend + Kafka consumer pods |
| AWS S3                           | ~$180/mo       | Backups, static assets        |
| AWS CloudFront + misc            | ~$124/mo       | CDN + data transfer           |
| **AWS Total**                    | **~$1,800/mo** |                               |
| Confluent Cloud (Kafka)          | $420/mo        | Annual contract               |
| Neon (PostgreSQL)                | $89/mo         | Primary DB                    |
| Redis (Upstash)                  | $35/mo         | Cache                         |
| Grafana Cloud                    | $299/mo        | Monitoring                    |
| Other SaaS                       | ~$400/mo       | Clerk, Typesense, Figma, etc. |
| **Total infra + SaaS**           | **~$3,043/mo** |                               |

---

## The Problem

TimescaleDB storage (EC2 EBS volume) is growing at **22% month-over-month**. At current rate:

| Month    | Projected EC2 Cost |
| -------- | ------------------ |
| Oct 2025 | $396               |
| Jan 2026 | $672               |
| Apr 2026 | $1,142             |
| Jul 2026 | $1,938             |

This is unsustainable at our burn rate. The driver is the 180-day raw event retention policy — we're storing 6 months of full event data per customer.

---

## Analysis: What Are We Actually Querying?

I pulled query patterns from the last 60 days of dashboard usage:

| Lookback Window    | % of Queries |
| ------------------ | ------------ |
| Last 24 hours      | 61%          |
| Last 7 days        | 22%          |
| Last 30 days       | 11%          |
| Last 90 days       | 5%           |
| Older than 90 days | 1%           |

**Key finding:** 99% of dashboard queries are within 90 days. Nobody is querying data older than 90 days in practice. The 180-day retention provides headroom nobody is using.

---

## Proposal: Tiered Retention

**Reduce raw event retention from 180 days to 90 days.**  
**Add 2-year retention for hourly aggregates** (pre-computed, small storage footprint).

This gives customers:

- 90 days of full granularity (matches 99% of actual usage)
- 2 years of hourly trend data for long-term analysis

**Projected storage savings:** ~40% reduction in TimescaleDB storage growth.

---

## Discussion

**Marcus:** I'm concerned about losing replay capability. If a customer has a bug in their SDK that we discover in month 4, we can't replay their data. I want to keep 180 days.

**Jordan:** The replay scenario is theoretical — we've never done it. And storage costs are real. We can add on-demand export before we delete data so customers have a self-service option.

**Priya:** Jordan's right. We optimize for the real use case, not the hypothetical one. Let's do 90 days.

**Marcus:** [accepted, reluctantly]

---

## Decisions

1. ✅ Reduce raw event retention to **90 days** (from 180 days). Effective after ClickHouse migration.
2. ✅ Add **2-year hourly aggregate** retention.
3. ✅ Notify existing customers before change goes live.
4. 🔄 Evaluate ClickHouse Cloud vs continued TimescaleDB scaling — Marcus to write RFC.

---

## Next Steps

| Action                            | Owner        | Due    |
| --------------------------------- | ------------ | ------ |
| Write retention policy v2 doc     | Jordan       | Oct 28 |
| Draft customer notification email | Tessa + Dana | Oct 28 |
| Write ClickHouse migration RFC    | Marcus       | Nov 15 |

---

_Notes by Jordan Okafor — October 20, 2025_
