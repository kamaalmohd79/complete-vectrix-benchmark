# Slack Corpus - Overview

## Summary

This folder contains the complete Slack workspace data for Vectrix Labs, Inc. — a fictional 10-person seed-stage API observability startup. The corpus covers the period **September 2025 through February 2026** and spans 7 public channels, 3 private DM conversations, and a manifest index file.

**Total messages: 170** (within the 130–170 target range)

---

## File Inventory

### Public Channels

| File               | Channel ID   | Messages | Date Range          | Description                                                                                       |
| ------------------ | ------------ | -------- | ------------------- | ------------------------------------------------------------------------------------------------- |
| `general.json`     | C07VECTRIX01 | 21       | Sep 2025 – Feb 2026 | Company-wide announcements, new hire welcomes, milestone celebrations                             |
| `engineering.json` | C07VECTRIX02 | 40       | Sep 2025 – Feb 2026 | Deployments, debugging, ClickHouse migration, Clerk auth migration, production incidents          |
| `product.json`     | C07VECTRIX03 | 21       | Sep 2025 – Feb 2026 | Feature debates, Meridian requirements, pricing restructure, Dashboard v2 comparison mode dispute |
| `sales.json`       | C07VECTRIX04 | 20       | Nov 2025 – Feb 2026 | Deal updates, Meridian pipeline, Cascade close, Ridgeway incident, deleted message aftermath      |
| `random.json`      | C07VECTRIX05 | 24       | Sep 2025 – Feb 2026 | Off-topic chat, food recommendations, editor wars, WeWork desk coordination                       |
| `ops.json`         | C07VECTRIX06 | 12       | Oct 2025 – Feb 2026 | PagerDuty alerts, AWS cost tracking, ClickHouse cost monitoring, payment failure                  |
| `design.json`      | C07VECTRIX07 | 8        | Nov 2025 – Feb 2026 | Dashboard v2 Figma shares, design review feedback, component spec questions                       |

### DM Conversations (`/dms/`)

| File         | Participants                    | Messages | Content Focus                                                                  |
| ------------ | ------------------------------- | -------- | ------------------------------------------------------------------------------ |
| `dm_01.json` | Marcus Webb ↔ Priya Chandran    | 10       | Frontend hiring comp band ($145K→$165K), Anya Chen final round decision        |
| `dm_02.json` | Tessa Moreno ↔ Caleb Whitford   | 8        | Ridgeway Financial over-promise cleanup, sales coaching on scoping commitments |
| `dm_03.json` | Sofia Gutierrez ↔ Liam Nakamura | 6        | Clerk auth migration approach review, PR review request                        |

### Manifest

| File                  | Description                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| `slack_manifest.json` | Index of all channels and DMs with message counts, date ranges, thread counts, file counts, and pinned counts |

---

## Channel-by-Channel Breakdown

### #general (21 messages)

The CEO broadcast channel. Priya announces new hires, company milestones, and financial updates. All 10 team members are introduced here across the timeline. Key moments include the Brainloop first-customer celebration (Oct 10), the Cascade Enterprise close (Jan 8), Raj's FTE conversion (Jan 15), the new pricing announcement (Jan 22), and the February MRR milestone ($18,200). Priya's voice here is short and declarative — never more than 2 sentences. She appears in other channels only through emoji reactions.

### #engineering (40 messages — most active channel)

The primary technical channel. Covers six major story arcs:

- **September bug hunt** — Kafka consumer lag causing delayed dashboard updates (Liam fixes in 2 days)
- **Brainloop anomaly detection false positives** — baseline calculation wrong at low traffic volumes (VEC-18)
- **ClickHouse migration RFC and execution** — Marcus's RFC, Jordan's staging cluster, Liam's data type mismatch bug, Dec 5 fix, Dec 10 cutover
- **Auth provider decision** — Marcus's Firebase vs Clerk vs Auth0 comparison, team consensus on Clerk
- **Dec 23 P1 incident** — Jordan's emotional vent about solo on-call during holidays, Marcus's empathetic response, Priya's ❤️ reaction
- **Feb 20 P2 incident** — ClickHouse maintenance window overlapping Cascade's peak traffic

### #product (21 messages)

Dana's home channel. Covers Meridian requirements scoping (VEC-47, VEC-52), the pricing restructure debate, the SSO-on-Growth-tier question (Tessa pushes back), and the ongoing Dashboard v2 comparison mode dispute between Dana (customers want it) and Marcus (backend isn't ready). The debate ends with comparison mode deferred to v2.1 — Dana accepts but isn't happy.

### #sales (20 messages)

Tessa narrates the pipeline in real time. Key moments: first Meridian call intel (Nov 20), Cascade close (Jan 8), the Ridgeway incident (Tessa's public note after Caleb's over-promise), the Meridian SOW arriving (Feb 10), and the Feb month-end pipeline wrap. Includes a deleted message aftermath: Caleb shared prospect deal details in the channel, Tessa's reply referencing it remains after the message was deleted.

### #random (24 messages)

The messiest channel. Includes editor wars (VSCode vs Neovim vs Cursor), food recommendations, WeWork desk coordination, Raj's first-day FTE comment, Sofia's accidental wrong-channel personal message, an unanswered question sitting 5+ days (Liam's dumpling request), and a typo correction (`*Tuesday not Thursday`).

### #ops (12 messages)

Jordan's operational log. AWS cost review flagging 22% month-over-month storage growth (Oct), the ClickHouse migration throughput test (52M events/day at 18ms p99 vs TimescaleDB's 340ms), PagerDuty alert notifications for both production incidents, Nina's Typesense payment failure handoff to Priya, and the ongoing ClickHouse cost monitoring discussion ($1,100/mo — Jordan watching for the $2K threshold).

### #design (8 messages)

Low-traffic channel. Raj shares Figma links, Dana and Sofia leave structured feedback. Covers the progression from early explorations (Nov) to the full 12-screen design review (Jan 30) to post-review component library updates (Feb 17). An unanswered question from Sofia about tab states sits without a reply for over a day before Raj responds.

---

## Edge Cases Implemented

All required edge cases from the instruction document are present:

| Edge Case                           | Where                           | Detail                                                                                                                                                                                                                                                              |
| ----------------------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Wrong channel correction**        | #engineering                    | Sofia posts a personal message ("hey can you pick up milk") immediately followed by "lol wrong chat sorry 😅" — gets laughing reactions                                                                                                                             |
| **Unresolved questions**            | #engineering, #product, #random | Liam's ClickHouse ZSTD compression question (no answer), Sofia's saved views filter question (no answer for 24h), Liam's dumpling request (sits 5 days before Caleb notices nobody responded)                                                                       |
| **Deleted message aftermath**       | #sales                          | Caleb's original message sharing prospect deal terms is gone; Tessa's reply ("hey can you delete that?") referencing it remains                                                                                                                                     |
| **Typo corrections**                | #engineering, #random           | Marcus corrects `*integration` after misspelling in a thread; Nina corrects `*tuesday not thursday` in #random                                                                                                                                                      |
| **Link rot / broken link**          | #ops                            | Jordan shares a Grafana dashboard link during the migration (`https://grafana.vectrix.io/d/xFz9kL7m/clickhouse-migration-monitor?...`) — this dashboard has since been reorganized; the URL remains in Slack history                                                |
| **Cross-platform references**       | Multiple                        | 7+ messages explicitly reference other sources: "per Dana's email," "see VEC-47," "tracking in VEC-52," "check the calendar invite," "PR #91 is up," "updated architecture spec is in Drive," "per my call notes"                                                   |
| **Emotional moment**                | #engineering                    | Dec 23 P1 incident: Jordan vents about being solo on-call at 11pm during holiday break. Marcus replies empathetically. Priya reacts with ❤️ but doesn't post. A few days later Priya posts in #general about formalizing on-call rotation — directly caused by this |
| **Celebration followed by problem** | #general → #engineering         | Brainloop signs Oct 10 (🎉 in #general). Two weeks later, Kevin Park's anomaly detection false positives land in #engineering — the celebration energy is entirely gone                                                                                             |

---

## Voice Consistency Notes

Each person sounds distinct throughout. Key voice markers applied:

- **Priya** — 1–2 sentences max in all channels. No greetings, no sign-offs. Appears in other channels exclusively through emoji reactions (✅, 👀, ❤️). Never @channel.
- **Marcus** — Numbered lists in longer messages, code in backticks, thorough thread replies. Posts technical context before asking for action.
- **Dana** — References Linear issues by number in every relevant message (VEC-47, VEC-52, VEC-65, VEC-82, VEC-84, VEC-89). Thinks out loud. Asks questions.
- **Liam** — Extremely concise. Single-sentence answers. Code blocks when relevant. Dry humor in #random.
- **Sofia** — Asks clarifying questions before building. Uses emoji. Active in #random. The wrong-channel message is hers.
- **Jordan** — Matter-of-fact deploy notifications. Cost-focused in #ops. The emotional vent is his — and it's earned by context.
- **Tessa** — Narrates deals in sentences. References dollar amounts. Celebrates wins, handles cleanup professionally.
- **Caleb** — Enthusiastic. Shorter than Tessa. Asks follow-up questions. Makes the mistake in #sales.
- **Nina** — Warm, logistical. Handles payment alerts, holiday coordination, social events.
- **Raj** — Quiet everywhere except #design. Posts Figma links. 🎨 emoji.

---

## Shared Files & Link Unfurls

| File                                      | Channel      | Type            | Uploaded By |
| ----------------------------------------- | ------------ | --------------- | ----------- |
| `backfill-error-dec2.log`                 | #engineering | Log file (text) | Liam        |
| `vectrix-enterprise-onepager-feb2026.pdf` | #sales       | PDF             | Tessa       |
| `pipeline-export-feb2026.csv`             | #sales       | CSV             | Caleb       |
| `office-selfie.jpg`                       | #random      | Image           | Nina        |

**Link unfurls (12 total):** Google Drive links (architecture spec, Q4 deck, auth comparison doc), GitHub PR links (PR #89 hotfix, PR #91 Clerk migration), Figma links (Dashboard v2 exploration, full review, component library), email unfurl (Typesense payment failure in #ops).

---

## Key Facts Traceable Through Slack

These facts are established in Slack and should match other corpus sources:

| Fact                                                                     | Source Message                             |
| ------------------------------------------------------------------------ | ------------------------------------------ |
| ClickHouse migration completed Dec 10, 2025                              | #engineering, Jordan's deploy notification |
| Data retention: 90 days raw, 2 years aggregated                          | #ops, Jordan's proposal thread             |
| Auth migration: Firebase → Clerk, started Jan 13                         | #engineering, Marcus's plan message        |
| Clerk migration 80% complete as of Feb 12                                | #engineering, Liam's status update         |
| Meridian requirements: SSO, SOC2 Type II, US residency, 1-year retention | #sales, Tessa's Nov 20 call debrief        |
| Dashboard v2 comparison mode deferred to v2.1                            | #product, Dana's Feb 17 announcement       |
| Dec 23 P1 incident: rate limiter, ~90 min downtime                       | #engineering, Jordan's incident thread     |
| Feb 20 P2 incident: ClickHouse maintenance, ~45 min stale data           | #ops and #engineering                      |
| ClickHouse Cloud cost: $1,100/mo as of Feb 2026                          | #ops, Jordan's cost note                   |
| Ridgeway over-promise: Caleb, Feb 7, Tessa corrected                     | #sales, Tessa's public note                |
| Anya Chen final round: Feb 27, strong recommendation                     | DM-01, Marcus to Priya                     |
| Frontend comp band raised to $145K–$165K                                 | DM-01, Priya approval                      |
