# Gmail Corpus — Overview

**Total threads: 18 | Total messages: 69 | Total attachments: 17**
**Date range: November 2025 – February 2026**
**Schema version: 2.0**

---

## Required JSON Schema

Every message contains all required fields per the spec:

| Field | Type | Notes |
|---|---|---|
| `message_id` | string | Unique ID, e.g. `msg_001` |
| `from` | `{name, email}` | Object, not string |
| `to` | array of `{name, email}` | |
| `cc` | array | Empty array `[]` if none |
| `bcc` | array | Empty array `[]` if none |
| `date` | ISO 8601 with timezone | e.g. `2026-01-28T14:32:00-08:00` |
| `subject` | string | |
| `body_text` | string | Plain text with signature appended |
| `body_html` | string | Simple `<p>` / `<br>` HTML |
| `attachments` | array | Each: `{filename, mime_type, size_bytes}` |
| `in_reply_to` | string or null | Parent `message_id` |
| `references` | array | Ancestor `message_id`s |
| `labels` | array | e.g. `["INBOX","IMPORTANT"]`, `["SENT"]` |
| `is_read` | boolean | |
| `is_starred` | boolean | |
| `signature` | string or null | Full name + title + email |

---

## Thread Inventory

### Internal Team Threads (6 threads)

| File | Subject | Msg | Edge Cases | Attachments |
|---|---|---|---|---|
| `thread_01_clickhouse_rfc.json` | ClickHouse Migration RFC — please review before Friday | 4 | — | — |
| `thread_02_dashboard_v2_missing_cc.json` | Dashboard v2 — product update + comparison mode | 4 | missing_cc, email_contradicts_slack | `dashboard-v2-product-brief.md` |
| `thread_03_pricing_reply_all.json` | Q1 pricing strategy — internal | 5 | reply_all_accident | `pricing-comparison.xlsx` |
| `thread_04_on_call_rotation.json` | On-call rotation — formalizing for Q1 | 3 | — | — |
| `thread_05_auth_migration.json` | Clerk auth migration — kicking off Jan 13 | 3 | — | — |
| `thread_06_forward_different_context.json` | Fwd: ClickHouse cost trajectory — thoughts? | 4 | forwarded_chain_different_context | — |

### Client / Partner Threads (3 threads)

| File | Subject | Msg | Edge Cases | Attachments |
|---|---|---|---|---|
| `thread_07_brainloop_ooo_incident.json` | Vectrix dashboard outage last night | 4 | ooo_auto_reply | — |
| `thread_08_cascade_onboarding.json` | Welcome to Vectrix — onboarding kickoff | 4 | *(see email_contradicts_slack in thread_02)* | `cascade-onboarding-agenda.docx` |
| `thread_09_meridian_discovery.json` | Vectrix evaluation — Meridian Health Systems | 4 | — | `meridian-sow-redlined.pdf`, `meridian-security-questionnaire.md`, `meridian-vendor-security-147q.xlsx` |

### Scheduling Threads (2 threads)

| File | Subject | Msg | Attachments |
|---|---|---|---|
| `thread_10_scheduling_meridian.json` | Meridian SOW review — scheduling | 4 | `meridian-sow-review-feb19.ics` |
| `thread_11_scheduling_anya_chen.json` | Interview scheduling — Sr. Frontend Engineer | 3 | `vectrix-interview-feb27-anya-chen.ics` |

### Vendor / Billing Threads (2 threads)

| File | Subject | Msg | Edge Cases | Attachments |
|---|---|---|---|---|
| `thread_12_talentbridge_fee_negotiation.json` | TalentBridge placement fee — renegotiation | 4 | vendor_negotiation_unresolved | `anya-chen-resume.pdf` |
| `thread_16_vanta_soc2.json` | Vanta — SOC 2 Type II readiness review | 3 | — | `vectrix-soc2-typeii-readiness.pdf` |

### Automated / Transactional Threads (2 threads)

| File | Subject | Msg | Edge Cases | Attachments |
|---|---|---|---|---|
| `thread_14_typesense_payment_failed.json` | [Typesense Cloud] Payment failed | 4 | failed_payment_followup_chain | `INV-2026-01-Typesense.pdf` |
| `thread_15_confluent_renewal.json` | [Confluent Cloud] Annual subscription renewal | 3 | — | `INV-2026-01-Confluent.pdf` |

### Recruiting Thread (1 thread)

| File | Subject | Msg | Attachments |
|---|---|---|---|
| `thread_13_derek_huang_decline.json` | Offer — Senior Frontend Engineer at Vectrix Labs | 4 | `derek-huang-offer-letter.pdf`, `employee-benefits-summary.pdf` |

### Investor / Board Threads (2 threads)

| File | Subject | Msg | Attachments |
|---|---|---|---|
| `thread_17_investor_amira_patel.json` | Vectrix Q4 update — Threshold Ventures | 4 | `investor-update-q4-2025.pdf`, `vectrix-dec23-incident-postmortem.pdf`, `metrics-dashboard.xlsx` |
| `thread_18_investor_sarah_intro.json` | Intro: Vectrix Labs // Luminary Health Engineering | 5 | — |

---

## Edge Cases (all 7 implemented)

| # | Edge Case | Thread | Summary |
|---|---|---|---|
| 1 | **Reply-all accident** | `thread_03` | Tessa sends internal pricing thread with Jason Liu (Ridgeway) accidentally CC'd. Caleb reply-alls exposing MRR targets, Meridian deal, pricing floor. Tessa warns Caleb privately; sends Jason a separate smoothing email framed as re-engagement. |
| 2 | **Missing CC looped in late** | `thread_02` | Dana sends v2 scope email to Priya + Marcus but not Tessa, who is actively selling comparison mode to Cascade and Ridgeway. Tessa pushes back. Dana apologizes and re-sends. Tessa's reply references her Slack thread with Marcus — cross-platform context gap. |
| 3 | **Email contradicts Slack decision** | `thread_02` | Dana's Jan 28 email says comparison mode is "confirmed for v2 launch." Feb 17 Slack (#engineering) defers it to v2.1. The email is stale/wrong — tests Authority/Version Resolution. |
| 4 | **OOO auto-reply** | `thread_07` | Priya is OOO Dec 23–Jan 2. Kevin Park (Brainloop) emails her about the Dec 23 P1 incident; gets the OOO auto-reply immediately. He then routes to Marcus directly and gets a full incident response. |
| 5 | **Forwarded chain with different context** | `thread_06` | Marcus forwards internal ClickHouse cost thread to David Kim (angel investor). Original thread has Jordan legitimately flagging $1,100/mo growth. Marcus adds "ignore Jordan's tone, he's just being Jordan" — reframing a valid concern as personality. David Kim receives this framing and advises accordingly. |
| 6 | **Vendor negotiation, unresolved** | `thread_12` | TalentBridge fee: Nina asks 15%, Amanda counters 16% (two placements) or 17% (single). Nina proposes 17% for Sr. Frontend now, revisit CSM later. Thread ends with no written agreement — fee is unresolved between 16–17%. |
| 7 | **Failed payment follow-up chain** | `thread_14` | Typesense Cloud automated failure email → Nina forwards to Priya ("can you update the card?") → Priya: "Done." → Typesense automated confirmation with `INV-2026-01-Typesense.pdf` attached. Full automated→human→automated chain. |

---

## Attachment Spec Compliance

Total attachments: **17** across **12 threads** (spec requires 6–9 minimum; exceeded for realism).

All attachments include `filename`, `mime_type`, and `size_bytes` per spec.

Attachments that correspond to `/drive/` files use identical filenames:

| Email attachment | Drive file |
|---|---|
| `dashboard-v2-product-brief.md` | `/drive/product/dashboard-v2-product-brief.md` |
| `meridian-security-questionnaire.md` | `/drive/sales/meridian-security-questionnaire.md` |
| `meridian-sow-redlined.pdf` | `/drive/contracts/meridian-sow-redlined.pdf` |
| `employee-benefits-summary.pdf` | `/drive/hr/employee-benefits-summary.pdf` |
| `metrics-dashboard.xlsx` | `/drive/finance/metrics-dashboard.xlsx` |

Automated invoice filenames follow vendor pattern: `INV-2026-01-Typesense.pdf`, `INV-2026-01-Confluent.pdf`.

---

## Cross-References to Other Corpus Sources

| Fact in Gmail | Also in Slack | Also in Drive |
|---|---|---|
| Dec 23 P1 incident, ~90 min, Jordan solo on-call (thread_07) | #ops, #engineering | sprint-review-2025-12-18.md |
| Comparison mode "confirmed for v2" Jan 28 — **contradicted** Feb 17 Slack (thread_02) | #engineering Feb 17 | dashboard-v2-feature-tracker.xlsx (VEC-82 deferred) |
| Caleb exposed Meridian + MRR to Jason Liu via reply-all (thread_03) | Slack DM Tessa↔Caleb | competitive-analysis.md |
| Meridian requirements: SSO, US residency, 12-month retention, SOC 2 (thread_09) | #sales | meridian-sow-redlined.pdf, meridian-security-questionnaire.md |
| Derek Huang declined at $145K; band raised to $145K–$165K (thread_13) | Slack DM Marcus↔Priya | sr-frontend-jd-v1.md → v2.md |
| Anya Chen final panel Feb 27; strong take-home (thread_11, thread_13) | — | interview-scorecard-anya-chen.xlsx |
| TalentBridge fee unresolved 16% vs 17% (thread_12) | — | hiring-plan-q1-2026.md (18% rate listed — adds a third conflicting data point) |
| Typesense payment failed Jan 18, resolved Jan 19 (thread_14) | — | — |
| $400K seed extension, ~18mo runway (thread_17) | — | investor-update-q4-2025.pptx |
| SOC 2 Type II committed June 30, 2026 (thread_09, thread_16) | — | meridian-sow-redlined.pdf §7 |
| ClickHouse cost $1,100/mo Feb 2026 (thread_06) | #engineering | architecture-spec-v2.md |
