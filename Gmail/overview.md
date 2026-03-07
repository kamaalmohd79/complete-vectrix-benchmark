# Gmail Corpus — Overview

## Summary

This folder contains the complete Gmail dataset for Vectrix Labs, Inc. The corpus covers **November 2025 through February 2026** and represents the full range of email traffic a seed-stage B2B SaaS startup accumulates: internal decision threads, client onboarding and escalations, vendor negotiations, scheduling back-and-forths, automated transactional emails, recruiting exchanges, and investor communication.

**Total threads: 18 | Total messages: 68**

All messages are internally consistent with the Slack corpus, Google Drive corpus, and with each other. Dates, salaries, deal values, ticket IDs, and decisions cross-reference correctly across all sources — with the exception of the intentional contradiction in Thread 02 (see Edge Cases).

---

## Thread Inventory

### Internal Team Threads (6 threads)

| File                                       | Subject                                                | Messages | Notes                                                                                                          |
| ------------------------------------------ | ------------------------------------------------------ | -------- | -------------------------------------------------------------------------------------------------------------- |
| `thread_01_clickhouse_rfc.json`            | ClickHouse Migration RFC — please review before Friday | 4        | Marcus sends RFC for Liam + Jordan review; Liam flags ZSTD(3), Jordan flags $800/mo cost; green light given    |
| `thread_02_dashboard_v2_missing_cc.json`   | Dashboard v2 — product update + comparison mode        | 4        | **EDGE CASES:** Missing CC (Tessa not included) + Email contradicts Slack decision                             |
| `thread_03_pricing_reply_all.json`         | Q1 pricing strategy — internal                         | 5        | **EDGE CASE:** Reply-all accident — Caleb exposes MRR targets and pricing floor to Jason Liu (Ridgeway)        |
| `thread_04_on_call_rotation.json`          | On-call rotation — formalizing for Q1                  | 3        | Post-Dec 23 incident; Marcus drafts rotation; Jordan requests holiday rule in writing                          |
| `thread_05_auth_migration.json`            | Clerk auth migration — kicking off Jan 13              | 3        | Marcus kicks off VEC-52/53; Liam flags Brainloop non-standard token; Sofia asks routing question               |
| `thread_06_forward_different_context.json` | Fwd: ClickHouse cost trajectory — thoughts?            | 4        | **EDGE CASE:** Marcus forwards internal cost thread to David Kim (angel investor), adds "ignore Jordan's tone" |

### Client / Partner Threads (3 threads)

| File                                    | Subject                                      | Messages | Notes                                                                                                                                                            |
| --------------------------------------- | -------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `thread_07_brainloop_ooo_incident.json` | Vectrix dashboard outage last night          | 4        | **EDGE CASE:** Priya OOO auto-reply; Kevin Park routes to Marcus; Marcus gives full incident response                                                            |
| `thread_08_cascade_onboarding.json`     | Vectrix onboarding — getting started         | 4        | Tone shift from warm welcome → pushback (Maria Chen demands comparison mode confirmation) → resolution. Dana's reply here contradicts the Feb 17 Slack deferral. |
| `thread_09_meridian_discovery.json`     | Vectrix evaluation — Meridian Health Systems | 3        | Dr. Alan Fong's initial inquiry; Priya answers all four requirements; Nina sends security questionnaire with the shared-externally doc attached                  |

### Scheduling Threads (2 threads)

| File                                  | Subject                          | Messages | Notes                                                                                                           |
| ------------------------------------- | -------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
| `thread_10_scheduling_meridian.json`  | Meridian SOW review — scheduling | 4        | "Does Feb 18 work?" → conflict → counter-proposal → confirmed Feb 19 11am PT. .ics attachment on final confirm. |
| `thread_11_scheduling_anya_chen.json` | Interview scheduling — Anya Chen | 3        | Marcus invites Anya for final panel; she proposes 10am–1pm combined on Feb 27; Nina confirms and sends .ics     |

### Vendor / Billing Threads (2 threads)

| File                                          | Subject                                    | Messages | Notes                                                                                                                                                                                  |
| --------------------------------------------- | ------------------------------------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `thread_12_talentbridge_fee_negotiation.json` | TalentBridge placement fee — renegotiation | 4        | **EDGE CASE:** Nina asks 15%, Amanda counters 16% (two placements) or 17% (one). Nina proposes 17% now + revisit for CSM. Thread ends without a written agreement — fee is unresolved. |
| `thread_16_vanta_soc2.json`                   | Vanta — SOC 2 Type II audit timeline       | 3        | Vanta CSM flags 6-month window, partial controls, auditor lead time; Nina forwards to Priya + Marcus; call scheduled                                                                   |

### Automated / Transactional Threads (2 threads)

| File                                      | Subject                                       | Messages | Notes                                                                                                                                      |
| ----------------------------------------- | --------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `thread_14_typesense_payment_failed.json` | [Typesense Cloud] Payment failed              | 4        | **EDGE CASE:** Automated failure email → Nina forwards to Priya ("can you update the card?") → Priya: "Done." → Typesense confirms payment |
| `thread_15_automated_transactional.json`  | [Confluent Cloud] Annual subscription renewal | 3        | Automated renewal notice for $5,040/year → Jordan forwards to Marcus → Marcus: "Renew. Same plan." + GitHub Actions CI failure mentioned   |

### Recruiting Threads (2 threads)

| File                                  | Subject                          | Messages | Notes                                                                                                                                               |
| ------------------------------------- | -------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `thread_13_derek_huang_decline.json`  | Offer — Senior Frontend Engineer | 4        | Nina extends offer at $145K; Derek declines (accepted $160K elsewhere); Marcus emails Priya to raise the band; Priya approves new range $145K–$165K |
| `thread_11_scheduling_anya_chen.json` | Interview scheduling — Anya Chen | 3        | _(also listed under Scheduling above)_ TalentBridge pipeline candidate; take-home praised; final panel Feb 27 confirmed                             |

### Investor / Board Threads (2 threads)

| File                                  | Subject                                            | Messages | Notes                                                                                                                                                                         |
| ------------------------------------- | -------------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `thread_17_investor_amira_patel.json` | Vectrix Q4 update — Threshold Ventures             | 4        | Amira Patel requests board materials; Priya sends preview (6.8x MRR) and full deck Jan 8 (Q4 board deck, post-mortem, metrics .xlsx attached); Amira asks follow-up questions |
| `thread_18_investor_sarah_intro.json` | Intro: Vectrix Labs // Luminary Health Engineering | 5        | Sarah Lindgren double-opts intro to Chloe Andrews (CTO, Luminary Health); Priya follows up; Chloe agrees to call; David Kim separate thread with strategic feedback           |

---

## Edge Cases

All 7 required edge cases are implemented:

| Edge Case                                         | Thread      | Detail                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Reply-all accident**                            | `thread_03` | Tessa starts an internal pricing strategy thread with Jason Liu (Ridgeway Financial) accidentally on the CC. Caleb replies-all, exposing Vectrix's $22K MRR target, the Meridian deal in progress, and internal discussion of Enterprise pricing flexibility. Tessa warns Caleb privately ("don't reply again"), then sends Jason a separate smoothing-over email with a re-engagement angle (new SSO, new pricing). Jason responds positively. |
| **Missing CC looped in late**                     | `thread_02` | Dana sends a v2 product scope email to Priya and Marcus but omits Tessa, who is actively selling comparison mode to Cascade and Ridgeway. Tessa pushes back ("why wasn't I CC'd?"). Dana apologizes and re-sends with Tessa added. Tessa's reply references her Slack conversation with Marcus — context that exists in Slack but not in the email thread.                                                                                      |
| **Email contradicts Slack decision**              | `thread_02` | Dana's Jan 28 email explicitly says comparison mode "IS in scope for v2 launch." On Feb 17 in Slack (#engineering), Marcus announces it's deferred to v2.1 due to backend complexity. The email is now stale and wrong — tests Authority/Version Resolution across corpus sources.                                                                                                                                                              |
| **Out-of-office auto-reply**                      | `thread_07` | Priya is OOO Dec 23–Jan 2. Kevin Park (Brainloop) emails her about the Dec 23 P1 incident and immediately receives her OOO auto-reply (with redirect to Marcus). Kevin routes to Marcus directly and gets a full incident response with root cause, remediation, and timeline.                                                                                                                                                                  |
| **Forwarded chain with different context**        | `thread_06` | Marcus forwards an internal ClickHouse cost concern thread to David Kim (angel investor) asking for advice. The original thread has Jordan legitimately flagging $1,100/mo cost growth. Marcus adds "ignore Jordan's tone, he's just being Jordan" — reframing a valid operational concern as personality. David Kim responds with useful advice and the conversation continues externally.                                                     |
| **Vendor negotiation back-and-forth, unresolved** | `thread_12` | TalentBridge placement fee negotiation. Nina asks for 15%; Amanda counters with 16% across both placements or 17% for the Sr. Frontend placement alone; Nina proposes 17% for now and to revisit when the CSM role opens. Thread ends without a written agreement — the actual fee for Anya Chen's placement is still unresolved between 16% and 17%.                                                                                           |
| **Failed payment follow-up chain**                | `thread_14` | Typesense Cloud sends an automated "payment failed — card declined" email to Nina. Nina forwards it to Priya with "can you update the card?" Priya responds "Done." the next morning. Typesense then sends a confirmation email with a PDF invoice attached. The full automated → human → automated chain is present in one thread.                                                                                                             |

---

## Attachments Present

Per the spec, at least 4–5 threads include attachments. The following threads have `.pdf`, `.ics`, `.docx`, or `.xlsx` attachments:

| Thread      | Attachment(s)                                                                                        |
| ----------- | ---------------------------------------------------------------------------------------------------- |
| `thread_02` | `dashboard-v2-product-brief.md` (Drive link)                                                         |
| `thread_08` | `vectrix-enterprise-onboarding-guide.pdf`                                                            |
| `thread_09` | `meridian-security-questionnaire.md` (shared Drive doc), `vectrix-soc2-type1-summary.pdf`            |
| `thread_10` | `meridian-sow-review-feb19.ics`                                                                      |
| `thread_11` | `vectrix-interview-feb27-anya-chen.ics`                                                              |
| `thread_13` | `derek-huang-offer-letter.pdf`, `employee-benefits-summary.pdf`                                      |
| `thread_14` | `typesense-invoice-jan2026.pdf`                                                                      |
| `thread_15` | `confluent-renewal-notice-2026.pdf`                                                                  |
| `thread_16` | `vectrix-soc2-typeii-readiness.pdf`                                                                  |
| `thread_17` | `vectrix-q4-board-deck.pdf`, `vectrix-dec23-incident-postmortem.pdf`, `vectrix-metrics-q4-2025.xlsx` |

---

## Cross-References to Other Corpus Sources

| Fact established in Gmail                                                                 | Also in Slack         | Also in Drive                                                 |
| ----------------------------------------------------------------------------------------- | --------------------- | ------------------------------------------------------------- |
| Dec 23 P1 incident, ~90 min downtime, Jordan solo on-call                                 | #ops, #engineering    | sprint-review-2025-12-18.md                                   |
| Comparison mode confirmed Jan 28 (Dana email) — **contradicted** by Feb 17 Slack deferral | #engineering Feb 17   | dashboard-v2-feature-tracker.xlsx (VEC-82 deferred)           |
| Caleb exposed Meridian deal + pricing to Jason Liu (Ridgeway)                             | Slack DM Tessa↔Caleb  | competitive-analysis.md                                       |
| Meridian requirements: SSO, US data residency, 12-month retention, SOC 2 Type II          | #sales                | meridian-sow-redlined.pdf, meridian-security-questionnaire.md |
| Derek Huang declined at $145K; band raised to $145K–$165K                                 | Slack DM Marcus↔Priya | sr-frontend-jd-v1.md → v2.md                                  |
| Anya Chen final panel Feb 27; praised take-home                                           | Slack DM Marcus↔Priya | interview-scorecard-anya-chen.xlsx                            |
| TalentBridge fee: 17% vs 16% unresolved                                                   | —                     | hiring-plan-q1-2026.md (18% rate listed)                      |
| Typesense payment failed Jan 18, resolved Jan 19                                          | —                     | metrics-dashboard.xlsx (noted as resolved)                    |
| David Kim angel extension: $400K seed extension, ~18mo runway                             | —                     | investor-update-q4-2025.pptx                                  |
| SOC 2 Type II: June 30, 2026 commitment to Meridian; auditor needed now                   | —                     | meridian-sow-redlined.pdf §7, vanta SOC2 doc                  |
