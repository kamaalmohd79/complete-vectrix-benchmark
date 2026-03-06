# Dashboard v2 — Product Brief

**Author:** Dana Reeves (dana@vectrix.io)  
**Created:** November 5, 2025  
**Last Modified:** January 8, 2026  
**Linear Project:** VEC-DASH-V2  
**Status:** In Progress — Target March 7, 2026  
**Design:** Raj Sundaram (Figma: https://www.figma.com/file/abc123/Dashboard-v2)

---

## Problem Statement

The current Vectrix dashboard (v1) was built fast to get to beta. It works, but it's ugly, inflexible, and doesn't reflect what customers actually need after using the product for a few months. In customer interviews (Brainloop, NovaBit, Cascade onboarding calls), three problems came up repeatedly:

1. **No customization.** Every user sees the same layout regardless of their use case. A platform team tracking 200 APIs has completely different needs from a 5-person startup.
2. **No saved views.** Customers set up the same filters every time they open the dashboard. There's no persistence.
3. **No comparison.** Customers want to compare endpoint performance across time periods — "show me this week vs last week" — and they can't.

Dashboard v1 also doesn't look good. Raj joined specifically to fix this. The v2 redesign is a full visual overhaul.

---

## Goals

1. Ship a dashboard that customers are proud to share with their team and stakeholders
2. Reduce "time to insight" — a customer should be able to go from "I have an alert" to "I know what's wrong" in under 60 seconds
3. Increase dashboard engagement (measured by sessions/week and time-on-site per session)
4. Unblock enterprise deals (Cascade required better UX before signing; Meridian will evaluate the dashboard during their trial)

---

## Scope — v2 (March 7, 2026)

### In Scope

**Core dashboard redesign**
- Full visual overhaul per Raj's Figma designs (12 screens)
- Responsive layout (1280px+ desktop-first)
- Vectrix Design System v1.0

**Endpoint detail view**
- Latency chart (P50/P95/P99 toggles, time range selector: 1h/6h/24h)
- Error rate and classification breakdown
- Request volume over time
- "Recent alerts" panel for the selected endpoint

**Saved views**
- Save any filter state (time range + endpoint selection + alert filters) as a named view
- Per-org view sharing (any org member can see and use shared views)
- Tab-based navigation between saved views in the main dashboard

**Customizable widgets (limited)**
- Move and resize widgets on the main dashboard canvas
- Persistent per-user layout stored in our DB

### Out of Scope (v2.1)

**Comparison mode (VEC-82)** — deferred. Requires backend query engine changes (multi-range aggregation). Marcus estimated 2-3 sprint points of backend work. Customers have asked for this; it's top of v2.1 backlog. Dana is not happy about deferring this.

---

## Non-Goals

- Mobile support (v3 or later)
- Distributed tracing view (not our product, not on roadmap)
- Dark mode (asked for by one user; low priority)

---

## Success Metrics

| Metric | Target | Measurement |
|---|---|---|
| Dashboard sessions/week (per active org) | +30% vs v1 baseline | Product analytics |
| Time to first alert insight | <60 seconds | User testing (target: 5 sessions pre-launch) |
| Customer satisfaction (net) | "Better" rating from 80%+ of existing customers | In-app survey at launch |

---

## Risks

| Risk | Likelihood | Mitigation |
|---|---|---|
| Raj is a contractor → FTE; calendar is unpredictable | Low (resolved — Raj is now FTE) | — |
| Frontend engineer bandwidth (Sofia is the only FE) | High | Sr. Frontend hire in progress |
| Marcus defers comparison mode | Happened | Accepted; committed to v2.1 timeline |
| Design scope creep | Medium | Design freeze per sprint; no changes to in-scope items mid-sprint |

---

## Key Decisions Log

| Date | Decision | Made By |
|---|---|---|
| Nov 5, 2025 | Scope defined; comparison mode in v2 | Dana |
| Jan 30, 2026 | Comparison mode deferred to v2.1 | Marcus (Dana disagreed) |
| Jan 30, 2026 | Core layout approved | Dana |
| Feb 3, 2026 | Target slipped to Feb 28 | Marcus + Dana |
| Feb 17, 2026 | Target slipped again to Mar 7 | Marcus |

---

*— Dana*
