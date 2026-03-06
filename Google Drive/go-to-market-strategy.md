# Go-to-Market Strategy — Vectrix Labs
## Mid-Market SaaS Engineering Teams

**Author:** Priya Chandran + Dana Reeves  
**Date:** October 2025  
**Status:** Working draft — reviewed quarterly

---

## ICP (Ideal Customer Profile)

**Primary:** Head of Engineering or VP Engineering at a B2B SaaS company with:
- 15–200 engineers total
- 10+ production APIs or microservices
- Using AWS (primary) or GCP/Azure
- Running ≥5M API calls/month
- Currently experiencing API reliability issues or growing too fast for manual monitoring

**Secondary:** Senior Platform or DevOps engineers at companies in the above range, who discover Vectrix through HN, engineering blogs, or GitHub.

**Anti-ICP:**
- Consumer apps (wrong buyer, wrong API patterns)
- Companies smaller than 10 engineers (no budget, no need)
- Companies larger than 500 engineers (Datadog is already embedded, hard to displace)
- Companies outside the US (no support bandwidth, regulatory complexity)

---

## Positioning

**One-liner:** "API observability for the team that can't afford to hire a Datadog consultant."

**Expanded:** Vectrix is purpose-built API observability for mid-market SaaS engineering teams. You get endpoint-level latency tracking, anomaly detection, and smart alerting — configured in a day, not a quarter.

**What we don't say:** We're not APM. We're not distributed tracing. We don't compete with Datadog on breadth. We compete on depth-for-APIs, time to value, and price.

---

## Channels

### 1. Bottom-Up Self-Serve (Primary — today)

The Growth tier ($19/mo) is designed to be discovered and adopted by individual engineers or small teams without involving procurement. Key channels:

- **Hacker News:** Where Kevin Park (Brainloop) found us. Engineering posts with traction drive trials. Priya or Marcus posts 2–3x/quarter.
- **Dev Twitter/X:** Marcus is active. Authentic technical threads > promotional posts.
- **GitHub integrations:** Our SDK is open source. GitHub stars → trial signups → conversion.
- **Word of mouth:** Our best channel after Brainloop. Kevin Park referred NovaBit.

### 2. Outbound (Secondary — Caleb's focus)

Caleb targets companies in our ICP using:
- LinkedIn Sales Navigator (title: Head of Eng, VP Eng, Platform Lead)
- Trigger events: job postings for "SRE" or "Platform Engineer" (they're feeling the pain)
- Company size: 50–500 employees, B2B SaaS, raised Series A+ within 24 months

**Current conversion metrics:**
- Outbound response rate: ~4% (improving; was 2% in September)
- Meeting → trial: ~60%
- Trial → paid: ~35%

### 3. Partner Referrals (Aspirational — 2026)

AWS Marketplace listing is in planning (Q2 2026). VCs referring portfolio companies is occasional but not systematic — Threshold Ventures has made 2 intros (Cascade, one prospect not yet closed).

---

## Pricing Strategy

**Philosophy:** Land cheap, expand with usage. The Growth tier ($19/mo) is a deliberate loss-leader to get engineering teams using the product. Revenue comes from Enterprise upgrades at deal size, not per-seat growth.

**Tier design (post Jan 22, 2026 update):**

| Tier | Price | Target |
|---|---|---|
| Free | $0 | Individual devs, evaluation |
| Growth | $19/mo | Small teams, 5–50 engineers |
| Enterprise | $99/mo + usage | 50+ engineers, compliance requirements |

**Key pricing decision:** We killed the old "Pro" tier ($29) because it confused prospects. "Growth" ($19) is cheaper, simpler, and converts better. Old Pro customers grandfathered through March 2026.

---

## Sales Motion

**Self-serve (Free → Growth):**
Automated email nurture sequence (6 emails over 14 days). Triggered when a user connects their first SDK. Caleb does personal outreach when a free user exceeds 80% of their event quota.

**Enterprise (Growth → Enterprise or direct Enterprise):**
1. Discovery call (Tessa or Caleb, 30 min) — understand use case and volume
2. Trial period (14 days Enterprise features)
3. Technical review (Marcus available for complex infra questions)
4. Procurement / legal (Nina routes to Orion Legal for contracts over $10K ACV)
5. Close

**Meridian deal motion:** Dr. Fong's team found us through a Hacker News thread in November. Priya got on a call within 48 hours. Trial in December. SOW in February. This is now our template for healthcare/regulated verticals.

---

## Competitive Positioning (Short Form)

Against **Datadog:** We're purpose-built and 5-10x cheaper for API-specific use cases. "Datadog is a 747 — great plane, but you're flying a Cessna route."

Against **Moesif:** Better anomaly detection, better alerting integrations. Moesif is stronger on API analytics (business metrics); we're stronger on ops/reliability.

Against **building in-house:** "You could build this. Brainloop tried. Kevin Park spent 6 weeks on a custom Kafka consumer + Grafana dashboards before trying Vectrix. We were up in 45 minutes."

---

## 2026 GTM Priorities

1. **Land Meridian** — validates healthcare/regulated vertical. Becomes a reference customer.
2. **AWS Marketplace listing** — unlocks enterprise procurement channels (budget exists in AWS committed spend).
3. **SDK v2** — dramatically reduces time-to-first-event. Key friction point in trial conversion is SDK setup.
4. **First CSM hire** — free Tessa to focus on new logos instead of managing existing relationships.

---

*Priya Chandran + Dana Reeves — October 2025, updated January 2026*
