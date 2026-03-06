# Vectrix Platform Architecture — v2

**Version:** 2  
**Author:** Marcus Webb (marcus@vectrix.io)  
**Created:** 2025-11-26  
**Last Modified:** 2025-12-10  
**Status:** CURRENT  
**Supersedes:** architecture-spec-v1.md

---

## 1. Overview

Vectrix ingests API traffic logs from customer environments, detects anomalies, and surfaces insights through a real-time dashboard. This document covers the technical architecture as of the ClickHouse migration (December 2025). Key changes from v1: analytics database migrated from TimescaleDB to ClickHouse Cloud; data retention policy updated to tiered model; auth provider migration from Firebase to Clerk in progress.

---

## 2. System Components

### 2.1 Ingestion Layer

Customer SDKs (Python, Node.js, Go) emit structured JSON events to the Vectrix ingestion endpoint. Events are immediately published to **Apache Kafka** (Confluent Cloud) and acknowledged. No synchronous processing happens in the hot path.

- **Kafka:** Confluent Cloud, 3 partitions per customer org
- **Message format:** JSON, schema version tracked in `schema_version` field
- **Max event size:** 64KB per event
- **Throughput target:** 50M+ events/day per customer (validated in ClickHouse staging)

### 2.2 Stream Processing

A pool of Go workers (EKS, us-east-1) consumes from Kafka and writes to ClickHouse Cloud. Workers are stateless; all state lives in the database. Workers were rewritten by Liam as part of the migration to use ClickHouse's native batch insert protocol instead of the PostgreSQL-compatible interface.

### 2.3 Analytics Database — ClickHouse Cloud

All API traffic events are stored in **ClickHouse Cloud** (managed, us-east-1).

- **Data retention:**
  - Raw API events: **90 days**
  - Hourly aggregates: **2 years**
- **Compression:** ZSTD per column
- **Engine:** ReplacingMergeTree for events table
- **Kafka partitions:** 3 per customer org
- **Write throughput (validated):** 52M events/day at p99 insert latency of 18ms
- **Schema:** `event_metadata` column uses `LowCardinality(String)` (not Nullable — nulls are coalesced to empty string on ingest)

> **Migration note:** Migrated from self-hosted TimescaleDB (EC2 r5.2xlarge) in November–December 2025. TimescaleDB kept read-only through January 10, 2026, then decommissioned. Migration RFC: see `clickhouse-migration-rfc.md`.

**Why ClickHouse over TimescaleDB:** TimescaleDB hit write throughput limits at ~50M events/day. ClickHouse Cloud delivers 10–20x write throughput with better compression and lower operational overhead as a managed service.

### 2.4 Primary Database — PostgreSQL

User accounts, organization settings, alert configurations, billing data, and API keys are stored in **PostgreSQL 16** (Neon, hosted).

- **Connection pooling:** PgBouncer (transaction mode), `max_connections = 50`
- **Migrations:** golang-migrate, tracked in `migrations/` directory

### 2.5 Cache — Redis

**Redis 7** (Upstash, serverless):

- Session storage (30-minute TTL)
- Rate limiting counters (sliding window, 1-minute buckets)
- Real-time dashboard state (WebSocket connection IDs per org)

### 2.6 Message Queue — Kafka

Confluent Cloud. Used for API event ingestion and internal fanout (alert evaluation, usage analytics). Annual contract, $5,000/year committed spend, signed September 2025.

### 2.7 Frontend — Dashboard SPA

**Next.js 14**, TypeScript, Tailwind CSS. Single-page application served via CloudFront CDN.

- **Auth:** Clerk (migration from Firebase in progress — see §5)
- **Real-time updates:** WebSocket connection to Go backend, proxied through CloudFront

### 2.8 Search — Typesense

Typesense Cloud. Powers endpoint search and log filtering. Synced from PostgreSQL on write.

---

## 3. Infrastructure

| Layer        | Technology                      | Notes                                            |
| ------------ | ------------------------------- | ------------------------------------------------ |
| Compute      | AWS EKS (us-east-1, us-west-2)  | Go backend pods, Kafka consumer pods             |
| Analytics DB | ClickHouse Cloud (managed)      | us-east-1; 90-day raw retention, 2yr aggregates  |
| Primary DB   | PostgreSQL 16 (Neon)            | Managed, us-east-1                               |
| Cache        | Redis 7 (Upstash)               | Serverless                                       |
| Queue        | Kafka (Confluent Cloud)         | Managed; 3 partitions/customer                   |
| CDN          | CloudFront                      | us-east-1 origin                                 |
| Storage      | S3                              | Backups, static assets, audit logs               |
| Auth         | Clerk (migration from Firebase) | Supports SAML SSO, org-level permissions         |
| Search       | Typesense Cloud                 | Managed                                          |
| Monitoring   | Grafana Cloud + PagerDuty       | Alerts → jordan.o (primary) → marcus (secondary) |
| CI/CD        | GitHub Actions                  | ~4 min build time                                |

---

## 4. Data Retention Policy

| Data Type         | Retention                     | Storage                      |
| ----------------- | ----------------------------- | ---------------------------- |
| Raw API events    | **90 days**                   | ClickHouse Cloud             |
| Hourly aggregates | **2 years**                   | ClickHouse Cloud             |
| User account data | Duration of account + 30 days | PostgreSQL (Neon)            |
| Audit logs        | 1 year                        | S3 (encrypted)               |
| Backups           | 30 days rolling               | S3 (encrypted, cross-region) |

> **Custom retention (Meridian):** Per-org retention configuration is in development (VEC-47). Meridian Health Systems requires 1-year raw event retention as a contract requirement.

---

## 5. Auth Migration — Firebase → Clerk

**Status:** In progress (80% complete as of February 12, 2026)

Firebase Auth lacked org-level permissions needed for enterprise customers. Meridian Health Systems requires SAML SSO, which Clerk supports natively.

- **Firebase:** Being phased out. 1 remaining customer (Brainloop) on Firebase tokens — non-standard bearer token format missing org claim. In progress.
- **Clerk:** All new customers. Org-level permissions, SAML/OIDC SSO, webhook support.
- **Migration window:** Both token formats accepted simultaneously by backend JWT middleware. Firebase fallback removed once all customers migrated.

Issues: VEC-52 (backend token validation), VEC-53 (frontend auth context).

---

## 6. Security

- All data in transit: TLS 1.2+
- All data at rest: AES-256 (AWS-managed keys, ClickHouse Cloud-managed keys)
- API keys: hashed (bcrypt) before storage, never logged
- Auth: Clerk-managed (new), Firebase (legacy during migration)
- Network: VPC with private subnets for database tier
- Data residency: us-east-1 and us-west-2 only (compliant with Meridian requirements)

---

## 7. Deployment

- **Deploy flow:** PR merged to `main` → auto-deploys to staging. Release tag → deploys to prod. Jordan manages prod deploys, posts in #engineering.
- **Hotfix flow:** `hotfix/` branch, expedited review (Marcus or Liam), deploy immediately.
- **Build time:** ~4 minutes
- **Current version:** v0.16.1

---

## 8. Incident Response

| Priority | Trigger              |Response  | Target Resolution |
| -------- | -------------------- | -------- | ----------------- |
| P1       | Service down         | Jordan + Marcus via PagerDuty (15-min acknowledge) | 2 hours           |
| P2       | Degraded performance | Jordan paged                                       | 8 hours           |
| P3       | Non-critical         | Linear issue, next sprint                          | Sprint cycle      |

Post-mortems required for all P1 and P2 incidents, published within 48 hours.

---

## 9. Known Gaps / Open Questions

- Comparison mode query engine (VEC-82): multi-range aggregation not yet supported; deferred to Dashboard v2.1.
- Distributed tracing: not yet implemented; debug requires manual log correlation.
- ClickHouse self-hosting evaluation: if Cloud costs exceed $2K/mo, Jordan wants to evaluate EC2. Marcus recommends waiting.

---

_Last reviewed by Marcus Webb, December 10, 2025_
