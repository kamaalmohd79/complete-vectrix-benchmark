# Sprint 12 Review

**Date:** February 17, 2026  
**Facilitator:** Marcus Webb  
**Attendees:** Priya Chandran, Marcus Webb, Dana Reeves, Liam Nakamura, Sofia Gutierrez, Jordan Okafor, Tessa Moreno, Caleb Whitford, Nina Kowalski (Raj — declined, PTO)  
**Notes:** Dana Reeves

---

## Sprint 12 Metrics

| Metric              | Value                                 |
| ------------------- | ------------------------------------- |
| Sprint              | 12                                    |
| Points committed    | 36                                    |
| Points completed    | 28                                    |
| Issues closed       | 10                                    |
| Issues carried over | 6                                     |
| Sprint dates        | Feb 3 – Feb 14 (Sprint 12 mid-review) |

Behind on velocity. Dashboard v2 frontend is the bottleneck.

---

## Dashboard v2 — Status Update (Sofia + Dana)

**Completion: ~85%.** Core dashboard, endpoint detail, and alert configuration complete. Remaining: saved views panel (VEC-88, ~80% done) and final QA pass.

**Comparison mode (VEC-82):** Formally deferred to v2.1. Decision is final. Dana accepted this but noted for the record: "Three customers have explicitly asked for this, and we've been saying 'coming soon' for two months. I want a firm v2.1 target date on the calendar before we launch v2." Marcus agreed to have a backend scope estimate by sprint 13.

**New v2 target: March 7, 2026** (slipped from Feb 28). Raj is on PTO this week. Sprint 13 will need to carry the finish line.

---

## Clerk Migration — COMPLETE (Liam)

✅ All 6 customers migrated to Clerk tokens as of February 12. Brainloop's non-standard token format was resolved by adding an org claim injection step in Liam's middleware before the Clerk JWT validator runs. Firebase fallback has been removed.

Migration is 100% complete. VEC-52 and VEC-53 closed.

---

## Meridian Update (Tessa)

Draft SOW received February 10. Terms:

- $2,800/mo Enterprise, 12-month term
- 99.9% uptime SLA with financial credits (5% per 0.1% below SLA, capped at 30%)
- 1-year raw event retention (requires VEC-47 completion)
- 60-day written notice for termination; immediate termination right for material breach

Orion Legal (James Hartley) is reviewing. Priya flagged two redline items: the termination-for-convenience clause and the SLA credit cap.

**Per-org retention config (VEC-47):** Liam estimates 2 weeks of backend work after Clerk is done. In progress.

**Target contract execution: March 15.** Very tight.

---

## February 20 Incident Review (Jordan)

ClickHouse Cloud maintenance window overlapped with Cascade's peak usage window (2–3 PM CT). Dashboard showed stale data for approximately 45 minutes. Maria Chen (Cascade) emailed within 20 minutes of the incident.

**Root cause:** ClickHouse Cloud schedules maintenance without customer notification. We didn't know the maintenance was happening.

**Remediation:**

1. Set up ClickHouse Cloud maintenance window alerts (done)
2. Added read-through cache for stale data fallback (in progress, Jordan)
3. Filed post-mortem and shared with Cascade

**SLA status for Cascade:** Cascade's SLA is 99.5% uptime, measured monthly. February uptime: 99.8% (45 min outage / 28-day month = 99.89%). We are not in breach. Tessa communicated this to Maria Chen.

---

## Hiring (Marcus)

Anya Chen final interview: February 27. Panel ready.

Marcus proposed offer: $158K base, 0.20% equity. Priya approved via DM.

If Anya accepts, target start: mid-March.

---

## Action Items

| Action                                     | Owner               | Due                |
| ------------------------------------------ | ------------------- | ------------------ |
| Complete saved views panel (VEC-88)        | Sofia               | Feb 28             |
| Final v2 QA pass                           | Sofia + Raj         | Mar 3              |
| VEC-47 per-org retention backend           | Liam                | Mar 1              |
| Backend scope for VEC-82 (comparison mode) | Marcus              | Sprint 13 planning |
| Anya Chen panel                            | Marcus, Dana, Priya | Feb 27             |
| Meridian SOW redlines back to Orion Legal  | Priya + Nina        | Feb 24             |

---

_Notes by Dana Reeves — February 17, 2026_  
_— Dana_
