# Sprint 11 Review

**Date:** January 30, 2026  
**Facilitator:** Marcus Webb  
**Attendees:** Priya Chandran, Marcus Webb, Dana Reeves, Liam Nakamura, Sofia Gutierrez, Jordan Okafor, Tessa Moreno, Caleb Whitford, Nina Kowalski, Raj Sundaram  
**Notes:** Dana Reeves

---

## Sprint 11 Metrics

| Metric              | Value                              |
| ------------------- | ---------------------------------- |
| Sprint              | 11                                 |
| Points committed    | 38                                 |
| Points completed    | 32                                 |
| Issues closed       | 12                                 |
| Issues carried over | 4 (VEC-82, VEC-84, VEC-88, VEC-91) |
| Sprint dates        | Jan 19 – Jan 31, 2026              |

---

## Dashboard v2 Status (Raj + Sofia + Dana)

**Core layout:** Approved. Raj presented final 12 screens. Dana signed off on endpoint detail view, latency chart, and alert configuration panel.

**Comparison mode (VEC-82):** Officially deferred to v2.1 per Marcus's recommendation. Requires multi-range query engine changes that aren't scoped for this sprint. Dana accepted the decision but put on record that three customers — Brainloop (Kevin Park), Cascade (Maria Chen), and one prospect — have explicitly asked for this. VEC-82 is top of v2.1 backlog. Marcus committed to architecture spec for the backend changes by sprint 13 planning.

**Saved views (VEC-88):** Sofia is implementing. ~70% complete. Tab state variants finalized with Raj (four states: default, hover, active, disabled). On track for v2 launch.

**Current v2 completion:** ~75%. New target: **February 28, 2026** (aggressive).

Marcus flagged this is still aggressive given carried-over frontend tickets.

---

## Clerk Migration Status (Liam)

80% complete as of today. 5 of 6 existing customers migrated to Clerk tokens. Remaining: Brainloop — non-standard bearer token format missing org claim. Liam estimates 1 week to resolve. Firebase fallback stays active until Brainloop is migrated.

---

## Meridian Update (Tessa)

Security questionnaire responses: Nina has completed 60%. 40+ questions still "In Progress" or blank — mostly in SOC 2 and encryption categories where we're waiting on the Vanta audit artifacts. Rachel Simmons has been patient but Tessa expects she'll follow up within 2 weeks.

SOW: Expected early February from Meridian's legal team. Priya and Nina will route to Orion Legal.

Dr. Fong update call: Scheduled February 3. Tessa to run.

---

## Hiring (Marcus)

Anya Chen completed take-home (Feb 24). Scored highly by Marcus and Sofia. Final round: **February 27**. Panel: Marcus (technical), Dana (product), Priya (culture/values). Decision expected Feb 28.

Current offer parameters: $158K base, 0.20% equity. Pending Priya approval (DM between Marcus and Priya).

---

## Pricing Transition (Caleb)

11 new Growth tier customers signed this month since the Jan 22 pricing launch. MRR now at $18,200. Existing customers on old Pro pricing honored through March.

Caleb flagged Ridgeway Financial is back in discovery after the February 7 scope miscommunication. Tessa has been in touch with Jason Liu to rebuild trust.

---

## Infra Update (Jordan)

ClickHouse Cloud costs: $1,100/mo. Jordan is tracking. No action at this level.

Feb 20 incident: ClickHouse maintenance window overlapped with Cascade's peak traffic. Dashboard showed stale data for ~45 minutes. Post-mortem filed. Maria Chen (Cascade) sent a concerned email — Tessa responded with an explanation and SLA credit. Jordan is configuring ClickHouse maintenance alerts to avoid recurrence.

---

## Action Items

| Action                                          | Owner               | Due                |
| ----------------------------------------------- | ------------------- | ------------------ |
| Resolve Brainloop Clerk token issue             | Liam                | Feb 7              |
| Complete security questionnaire (remaining 40%) | Nina                | Feb 14             |
| Anya Chen final interview                       | Marcus, Dana, Priya | Feb 27             |
| Architecture spec for VEC-82 backend            | Marcus              | Sprint 13 planning |
| Configure ClickHouse maintenance window alerts  | Jordan              | Feb 7              |

---

_Notes by Dana Reeves — January 30, 2026_  
_— Dana_
