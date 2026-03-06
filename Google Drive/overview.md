# Google Drive Corpus — Overview

## Summary

This folder contains the complete Google Drive dataset for Vectrix Labs, Inc. — a fictional 10-person seed-stage API observability startup. The corpus spans **September 2025 through February 2026** and covers every document type a real startup would accumulate: technical specs, meeting notes, contracts, spreadsheets, presentations, HR docs, planning docs, and images.

**Total files: 35** in `drive/` + **2 files** in `notion/` = **37 files total**

All files are internally consistent with the Slack corpus and with each other. Version numbers, salaries, ticket IDs, dates, and decisions cross-reference correctly across sources.

---

## File Inventory by Type

### Specs / Technical Docs (9 files)

| File                          | Format | Status        | Notes                                                                           |
| ----------------------------- | ------ | ------------- | ------------------------------------------------------------------------------- |
| `architecture-spec-v1.md`     | .md    | SUPERSEDED    | Old architecture: TimescaleDB, 180-day retention, Firebase Auth                 |
| `architecture-spec-v2.md`     | .md    | CURRENT       | New architecture: ClickHouse Cloud, tiered retention, Clerk migration           |
| `data-retention-policy-v1.md` | .md    | SUPERSEDED    | 180-day flat retention for all raw events                                       |
| `data-retention-policy-v2.md` | .md    | CURRENT       | 90-day raw + 2-year aggregated; references ClickHouse TTL                       |
| `sr-frontend-jd-v1.md`        | .md    | SUPERSEDED    | Salary band $140K–$155K; no equity range                                        |
| `sr-frontend-jd-v2.md`        | .md    | CURRENT       | Raised to $145K–$165K; 0.15–0.25% equity; Anya Chen context                     |
| `clickhouse-migration-rfc.md` | .md    | APPROVED      | Marcus's RFC for migrating from TimescaleDB; approved Nov 26                    |
| `auth-provider-comparison.md` | .md    | DECISION MADE | Firebase vs Clerk vs Auth0; Clerk chosen Dec 12                                 |
| `sdk-v2-rfc.md`               | .md    | DRAFT         | Liam's RFC for SDK rewrite; contains TBD sections (Go approach, docs ownership) |

### Meeting Notes (5 files)

| File                              | Format | Date         | Notes                                                                               |
| --------------------------------- | ------ | ------------ | ----------------------------------------------------------------------------------- |
| `sprint-review-2025-12-18.md`     | .md    | Dec 18, 2025 | Q4 sprint review: ClickHouse complete, Meridian in negotiation, Dashboard v2 behind |
| `infra-cost-review-2025-10-20.md` | .md    | Oct 20, 2025 | Jordan's cost analysis that led to 90-day retention decision; query pattern data    |
| `sprint-review-2026-01-30.md`     | .md    | Jan 30, 2026 | Sprint 11: comparison mode deferred, Clerk 80%, Anya Chen final round               |
| `sprint-review-2026-02-17.md`     | .md    | Feb 17, 2026 | Sprint 12: Clerk complete, Dashboard v2 85%, Meridian SOW received                  |
| `all-hands-2026-01-06.md`         | .md    | Jan 6, 2026  | **EDGE CASE:** Agenda template pasted; no notes ever filled in                      |

### Contracts / Legal (4 files)

| File                            | Format | Status            | Notes                                                                                               |
| ------------------------------- | ------ | ----------------- | --------------------------------------------------------------------------------------------------- |
| `meridian-sow-redlined.pdf`     | .pdf   | DRAFT — IN REVIEW | **EDGE CASE:** Orion Legal redlines on SLA credit cap (§5.1) and termination-for-convenience (§6.1) |
| `cascade-nda.pdf`               | .pdf   | EXECUTED          | Clean mutual NDA; signed Dec 15, 2025 by Priya + Maria Chen                                         |
| `raj-offer-letter.pdf`          | .pdf   | EXECUTED          | Raj Sundaram FTE conversion; $95K base, 0.15% equity; accepted Jan 14, 2026                         |
| `employee-benefits-summary.pdf` | .pdf   | CURRENT           | Full 2026 benefits guide: health, dental, 401k, PTO, stipends, equity                               |

### Spreadsheets (6 files)

| File                                 | Format | Notes                                                                                  |
| ------------------------------------ | ------ | -------------------------------------------------------------------------------------- |
| `pricing-v1.xlsx`                    | .xlsx  | Old pricing: Free/$0, Pro/$29, Enterprise/$149 flat                                    |
| `pricing-v2.xlsx`                    | .xlsx  | New pricing (Jan 22, 2026): Free/$0, Growth/$19, Enterprise/$99+usage                  |
| `q1-budget-2026.xlsx`                | .xlsx  | **EDGE CASE:** SUM formula excludes Nina Kowalski's row — total understated by $85K/yr |
| `metrics-dashboard.xlsx`             | .xlsx  | MRR + customer list Sep 2025–Feb 2026; 14 customers, $18,200 MRR                       |
| `dashboard-v2-feature-tracker.xlsx`  | .xlsx  | 15 features tracked; 8 done, 1 in-progress, 5 not started, 1 deferred (VEC-82)         |
| `interview-scorecard-anya-chen.xlsx` | .xlsx  | Scorecard for Sr. Frontend final round; strong hire recommendation                     |

### Presentations (3 files)

| File                               | Format | Slides | Notes                                                                                                                   |
| ---------------------------------- | ------ | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| `q4-allhands-2025.pptx`            | .pptx  | 9      | **EDGE CASE:** DRAFT — DO NOT DISTRIBUTE watermark; slide 8 says "[Raj to add dashboard screenshots]" — never completed |
| `investor-update-q4-2025.pptx`     | .pptx  | 6      | Q4 investor update sent to Threshold Ventures Dec 20; clean and final                                                   |
| `sprint-review-deck-sprint12.pptx` | .pptx  | 5      | Sprint 12 review: Dashboard v2 status, Clerk complete, Meridian SOW, Anya Chen                                          |

### Planning & Strategy Docs (4 files)

| File                            | Format | Author         | Notes                                                                                          |
| ------------------------------- | ------ | -------------- | ---------------------------------------------------------------------------------------------- |
| `dashboard-v2-product-brief.md` | .md    | Dana Reeves    | Full product brief: scope, non-goals, risks, decision log; comparison mode deferral documented |
| `competitive-analysis.md`       | .md    | Dana Reeves    | Vectrix vs Datadog, Moesif, Honeycomb; deal-level competitive intel from Caleb + Tessa         |
| `hiring-plan-q1-2026.md`        | .md    | Priya Chandran | Sr. Frontend (active) and CSM (contingent on Meridian); comp bands, recruiter details          |
| `go-to-market-strategy.md`      | .md    | Priya + Dana   | ICP definition, positioning, channels, pricing philosophy, 2026 GTM priorities                 |

### HR / Ops Docs

See Contracts / Legal above — `raj-offer-letter.pdf` and `employee-benefits-summary.pdf` cover the HR/ops file requirement.

### Sales Docs (1 file)

| File                                 | Format | Notes                                                                                                                                                                |
| ------------------------------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `meridian-security-questionnaire.md` | .md    | **EDGE CASE:** Shared externally with Rachel Simmons (Meridian); her margin comments on Q47 (SLA cap) and Q61 (ClickHouse subprocessor) are embedded in the document |

### Images (2 files)

| File                          | Format | Notes                                                                                                   |
| ----------------------------- | ------ | ------------------------------------------------------------------------------------------------------- |
| `architecture-diagram-v2.png` | .png   | Full system architecture post-ClickHouse migration; all layers from SDK → Kafka → ClickHouse → frontend |
| `dashboard-v2-screenshot.png` | .png   | Dashboard v2 UI mockup; latency chart, endpoint list, saved views panel, alert feed                     |

---

## Notion Files (`notion/`)

| File                              | Notes                                                                                                                                                                                                                                                                          |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `data-retention-policy-notion.md` | **EDGE CASE:** Older Notion version of the retention policy. Contains Section 5 (Customer Data Export rights, SLAs, portability) that was NOT carried over to the Google Drive version during migration. Jordan flagged this in a Notion comment on Oct 16 — still unresolved. |
| `notion_manifest.json`            | Index of Notion pages with metadata, author, dates, and the comment thread about the missing section.                                                                                                                                                                          |

---

## Version Conflict Pairs

All four required version-conflict pairs are present, clearly labeled, and contain genuinely conflicting information:

| Pair                              | What Changed                                                                                | Why It Matters for Retrieval                               |
| --------------------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| `architecture-spec-v1` → `v2`     | TimescaleDB → ClickHouse; 180-day retention → 90-day raw + 2yr aggregated; Firebase → Clerk | Tests whether AI picks the current architecture spec       |
| `data-retention-policy-v1` → `v2` | 180-day flat → tiered (90-day raw, 2yr aggregated)                                          | Cross-references infra cost review doc                     |
| `pricing-v1` → `v2`               | Pro $29/mo → Growth $19/mo; Enterprise $149 flat → $99 + usage; tier renamed                | Tests authority / version resolution on pricing queries    |
| `sr-frontend-jd-v1` → `v2`        | Salary $140K–$155K → $145K–$165K; equity range added                                        | Reflects Derek Huang declining; connects to DM-01 in Slack |

---

## Edge Cases Implemented

All 6 required edge cases from the spec are present:

| Edge Case                              | File                                     | Detail                                                                                                                                                                                                                                                                                |
| -------------------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Formula error**                      | `q1-budget-2026.xlsx`                    | SUM in payroll total covers rows 6:14 only — Nina Kowalski (row 15) is excluded. Budget total is understated by $85,000/year ($21,250/quarter).                                                                                                                                       |
| **Empty meeting notes**                | `all-hands-2026-01-06.md`                | Jan 6 all-hands agenda template was pasted into a new doc, never filled in. Sections read: "[Notes were not taken during this meeting.]"                                                                                                                                              |
| **Redlined contract**                  | `meridian-sow-redlined.pdf`              | Orion Legal (James Hartley) redlines two clauses in the Meridian SOW: SLA credit cap raised from 30% to 100% (§5.1), and termination-for-convenience removed from Initial Term (§6.1). Comments explain the reasoning.                                                                |
| **DRAFT watermark + incomplete slide** | `q4-allhands-2025.pptx`                  | All 9 slides have "DRAFT — DO NOT DISTRIBUTE" watermark. Slide 8 (Dashboard v2 Preview) has only placeholder text: "[Raj to add dashboard screenshots]" inside a dashed box.                                                                                                          |
| **Shared externally with comment**     | `meridian-security-questionnaire.md`     | Document is shared with Rachel Simmons (VP IT Security, Meridian). Her margin comments are embedded — she flags Q47 (SLA credit cap, same issue as the SOW) and Q61 (needs ClickHouse Cloud underlying provider detail).                                                              |
| **Notion-only section**                | `notion/data-retention-policy-notion.md` | The Notion version of the retention policy contains a full Section 5 (Customer Data Export rights, pre-deletion export SLA, portability guarantees) that was never migrated to the Google Drive version when the doc was moved. Jordan flagged it in a comment; Marcus deprioritized. |

---

## Cross-References to Other Corpus Sources

These facts are established in Drive and must match other corpus sources:

| Fact                                                       | Drive Source                                                         | Also Appears In                       |
| ---------------------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------- |
| ClickHouse migration completed Dec 10, 2025                | `architecture-spec-v2.md`, `clickhouse-migration-rfc.md`             | Slack #engineering                    |
| Data retention: 90-day raw + 2-year aggregated             | `data-retention-policy-v2.md`                                        | Slack #ops, architecture-spec-v2      |
| Auth migration: Firebase → Clerk, decision Dec 12          | `auth-provider-comparison.md`                                        | Slack #engineering                    |
| Salary band raised to $145K–$165K                          | `sr-frontend-jd-v2.md`                                               | Slack DM-01 (Marcus ↔ Priya)          |
| Pricing change Jan 22: Growth $19, Enterprise $99+usage    | `pricing-v2.xlsx`                                                    | Slack #general, #product              |
| Meridian SOW: $2,800/mo, 99.9% SLA, 1-yr retention         | `meridian-sow-redlined.pdf`                                          | Slack #sales, #product                |
| Raj offer: $95K base, 0.15% equity, Jan 15 FTE             | `raj-offer-letter.pdf`                                               | Slack #general, #random               |
| Comparison mode (VEC-82) deferred to v2.1                  | `dashboard-v2-feature-tracker.xlsx`, `dashboard-v2-product-brief.md` | Slack #product                        |
| Anya Chen: final round Feb 27, offer $158K/0.20%           | `interview-scorecard-anya-chen.xlsx`                                 | Slack DM-01, sprint-review-2026-02-17 |
| Cascade NDA executed Dec 15                                | `cascade-nda.pdf`                                                    | Slack #sales                          |
| Q1 budget total is off by $85K (Nina excluded from SUM)    | `q1-budget-2026.xlsx`                                                | Nowhere else — Drive-only discrepancy |
| Section 5 (Export rights) missing from Drive retention doc | `notion/data-retention-policy-notion.md`                             | Notion only                           |
