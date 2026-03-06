# Sprint 7 Review + Q4 All-Hands

**Date:** December 18, 2025  
**Facilitator:** Marcus Webb  
**Attendees:** Priya Chandran, Marcus Webb, Dana Reeves, Liam Nakamura, Sofia Gutierrez, Jordan Okafor, Tessa Moreno, Caleb Whitford, Nina Kowalski, Raj Sundaram  
**Notes:** Dana Reeves

---

## Sprint 7 Metrics

| Metric              | Value                      |
| ------------------- | -------------------------- |
| Points committed    | 37                         |
| Points completed    | 34                         |
| Issues closed       | 14                         |
| Issues carried over | 3 (VEC-62, VEC-63, VEC-68) |
| Avg build time      | 3m 52s                     |

**Velocity note:** 3 carried over issues were all Dashboard v2 frontend work. Sofia flagged that the Figma handoff came late in the sprint — design was still iterating in week 2, which blocked implementation. Marcus flagged this was consistent with the last two sprints.

---

## Q4 Company Metrics (Priya)

| Metric               | Q4 Result   |
| -------------------- | ----------- |
| MRR                  | $15,400     |
| Paying customers     | 14          |
| Free-tier users      | 38          |
| Uptime               | 99.7%       |
| ClickHouse migration | ✅ Complete |

Priya's note: "We shipped a lot in Q4. ClickHouse, Clerk decision, Cascade signed, Meridian in negotiation. But we're behind on Dashboard v2 and the frontend hire is taking longer than expected."

---

## Q1 2026 Targets (Priya)

- MRR: $22,000 by March 31
- Dashboard v2: launch by February 28
- Meridian: signed contract by March 15
- Sr. Frontend Engineer: offer accepted
- SOC 2 Type II: audit scheduled

---

## Dashboard v2 Status (Dana + Raj)

Raj presented the 12-screen Figma designs. Core layout approved. Two open debates:

**Comparison mode (VEC-82):** Dana wants it in v2. Marcus says the query engine changes needed (multi-range aggregation) are 2-3 sprint points of backend work and shouldn't block the core v2 launch. Dana disagrees. **Decision deferred to next sprint planning.** Dana is visibly not happy about this.

**Saved views:** In scope for v2. Sofia is implementing. Currently ~60% complete.

---

## Meridian Status (Tessa)

- Security questionnaire (147 questions) received January 20 — Nina coordinating responses
- SOC 2 Type II gap: Vectrix has Type I. Priya committed to Q2 Type II audit on the call with Dr. Fong
- Draft SOW expected early February
- Clerk migration (SSO prerequisite) at 80% — Liam estimates 1 more week
- Per-org retention config (VEC-47) needs to be done before contract execution

---

## ClickHouse Migration Post-Mortem Highlights (Liam + Jordan)

Migration complete as of Dec 10. One issue: `event_metadata` data type mismatch (varchar vs LowCardinality). Fixed Dec 5. Final outcome: 10.4M rows migrated, 0 errors.

TimescaleDB kept read-only through Jan 10, 2026.

---

## Hiring Update (Marcus + Priya)

Sr. Frontend Engineer search:

- 1 candidate declined (Derek Huang, comp too low at $145K)
- 1 candidate ghosted after take-home
- 1 candidate in final rounds (Anya Chen, ex-Vercel)

Anya's final round scheduled Feb 27. Comp band raised to $145K–$165K base + 0.15–0.25% equity.

---

## Holiday + On-Call (Nina + Jordan)

Company shutdown: Dec 23 – Jan 2. Jordan on-call for P1 infra. Liam backup for emergencies.

**Action item (Marcus):** Formalize on-call rotation for Q1. Jordan shouldn't be the only person paged nights. Priya to post in #general after the break.

---

## Retro Summary (async, posted in #engineering)

**What went well:** ClickHouse migration shipped cleanly after the Dec 5 fix. Cascade onboarding went smoothly. Raj's designs are getting better.

**What didn't:** Frontend velocity blocked by late design handoffs. Hiring taking longer than expected. Sprint planning estimates still off for frontend work.

**Actions:** Earlier design freeze (no changes after sprint starts); Marcus to post hiring postmortem after Anya decision.

---

_Notes by Dana Reeves — December 18, 2025_  
_— Dana_
