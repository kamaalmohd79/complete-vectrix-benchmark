# Vectrix Platform Architecture — v1

**Version:** 1  
**Author:** Marcus Webb (marcus@vectrix.io)  
**Created:** 2025-07-10  
**Last Modified:** 2025-09-15  
**Status:** SUPERSEDED — see architecture-spec-v2.md

---

## 1. Overview

Vectrix ingests API traffic logs from customer environments, detects anomalies, and surfaces insights through a real-time dashboard. This document covers the technical architecture as of the public beta launch (September 2025).

---

## 2. System Components

### 2.1 Ingestion Layer

Customer SDKs (Python, Node.js, Go) emit structured JSON events to the Vectrix ingestion endpoint. Events are immediately published to **Apache Kafka** (Confluent Cloud) and acknowledged. No synchronous processing happens in the hot path.

- **Kafka:** Confluent Cloud, 3 partitions per customer org
- **Message format:** JSON, schema version tracked in `schema_version` field
- **Max event size:** 64KB per event
- **Throughput target:** 10M events/day per customer (soft cap)

### 2.2 Stream Processing

A pool of Go workers (EKS, us-east-1) consumes from Kafka and writes to the analytics database. Workers are stateless; all state lives in the database.

### 2.3 Analytics Database — TimescaleDB

All API traffic events are stored in **TimescaleDB**, self-hosted on an EC2 instance (r5.2xlarge, us-east-1).

- **Data retention:** 180 days for all raw event data
- **Compression:** TimescaleDB native compression, enabled after 7 days
- **Hypertable chunk interval:** 1 day
- **Indexes:** (org_id, endpoint_id, timestamp) composite index on the events table
- **Replication:** Single primary, daily snapshots to S3

> **TODO:** Evaluate read replica for dashboard query load as customer count grows. Currently dashboard queries hit the primary directly, which is fine at <20 customers but will need attention.

**Why TimescaleDB:** Strong PostgreSQL compatibility, familiar SQL interface, time-series compression, and good ecosystem tooling. The team has PostgreSQL expertise.

### 2.4 Primary Database — PostgreSQL

User accounts, organization settings, alert configurations, billing data, and API keys are stored in **PostgreSQL 16** (Neon, hosted).

- **Connection pooling:** PgBouncer (transaction mode)
- **Migrations:** golang-migrate, tracked in `migrations/` directory

### 2.5 Cache — Redis

**Redis 7** (Upstash, serverless) handles:

- Session storage (30-minute TTL)
- Rate limiting counters (sliding window, 1-minute buckets)
- Real-time dashboard state (WebSocket connection IDs per org)

### 2.6 Message Queue — Kafka

See §2.1. Kafka is also used for internal event fanout (alert evaluation, usage analytics aggregation).

### 2.7 Frontend — Dashboard SPA

**Next.js 14**, TypeScript, Tailwind CSS. Single-page application served via CloudFront CDN.

- **Auth:** Firebase Auth (Google-managed, email/password + Google OAuth)
- **Real-time updates:** WebSocket connection to the Go backend, proxied through CloudFront

### 2.8 Search — Typesense

Powers endpoint search and log filtering in the dashboard. Typesense Cloud instance, synced from PostgreSQL on write.

---

## 3. Infrastructure

| Layer        | Technology                     | Notes                                |
| ------------ | ------------------------------ | ------------------------------------ |
| Compute      | AWS EKS (us-east-1, us-west-2) | Go backend pods, Kafka consumer pods |
| Analytics DB | TimescaleDB (EC2 r5.2xlarge)   | Self-hosted, us-east-1               |
| Primary DB   | PostgreSQL 16 (Neon)           | Managed, us-east-1                   |
| Cache        | Redis 7 (Upstash)              | Serverless                           |
| Queue        | Kafka (Confluent Cloud)        | Managed                              |
| CDN          | CloudFront                     | us-east-1 origin                     |
| Storage      | S3                             | Backups, static assets               |
| Auth         | Firebase Auth                  | Google-managed                       |
| Search       | Typesense Cloud                | Managed                              |
| Monitoring   | Grafana Cloud + PagerDuty      | Alerts → jordan.o                    |
| CI/CD        | GitHub Actions                 | ~4 min build time                    |

---

## 4. Data Retention Policy

| Data Type         | Retention                     |
| ----------------- | ----------------------------- |
| Raw API events    | **180 days**                  |
| User account data | Duration of account + 30 days |
| Audit logs        | 1 year (S3)                   |
| Backups           | 30 days rolling (S3)          |

---

## 5. Security

- All data in transit: TLS 1.2+
- All data at rest: AES-256 (AWS-managed keys)
- API keys: hashed (bcrypt) before storage, never logged
- Auth: Firebase-managed, no Vectrix-stored passwords
- Network: VPC with private subnets for database tier; EKS nodes in private subnets

---

## 6. Deployment

- **Deploy flow:** PR merged to `main` → auto-deploys to staging. Release tag (e.g., `v0.12.0`) → deploys to prod. Jordan manages prod deploys.
- **Hotfix flow:** Branch off `main`, `hotfix/` prefix, expedited review (Marcus or Liam), deploy immediately.
- **Build time:** ~4 minutes (GitHub Actions)
- **Current version:** v0.12.0

---

## 7. Open Questions / Known Gaps

- TimescaleDB read load: dashboard queries at >50 concurrent users may cause latency. Need to monitor as customer count grows.
- No distributed tracing yet — debugging latency issues requires manual log correlation.
- Firebase Auth lacks org-level permissions model — will become a problem when enterprise customers require SAML SSO.

---

_Last reviewed by Marcus Webb, September 15, 2025_
