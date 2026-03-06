# Vectrix Security Questionnaire Response

## Meridian Health Systems — Vendor Security Review

**Document Status:** SHARED EXTERNALLY — Meridian Health Systems  
**Shared with:** Rachel Simmons (rachel.simmons@meridianhealth.com), VP IT Security  
**Shared by:** Nina Kowalski (nina@vectrix.io)  
**Shared date:** January 22, 2026  
**Last modified:** February 3, 2026

> **[EXTERNAL COMMENT — Rachel Simmons, rachel.simmons@meridianhealth.com, Jan 24, 2026]:**  
> Hi Nina — thanks for getting this over so quickly. We've reviewed the first 60 questions and most look good. I have a few open items I'm flagging here. Most important: **Question 47 (SLA Credits)** — your current answer references a 30% monthly cap on credits. Our standard vendor requirements require either uncapped credits or a minimum 100% monthly fee cap for SLA breaches. Can you clarify whether Vectrix is willing to adjust this in the SOW? We'd like to resolve this before routing the contract to our legal team. I've flagged the specific questions below. Happy to jump on a 30-min call if easier.

---

## SECTION A: Organization & Governance

**Q1. Provide your company's legal name, jurisdiction of incorporation, and primary business address.**  
Vectrix Labs, Inc. Delaware corporation. Primary address: 548 Market St, PMB 73344, San Francisco, CA 94104.

**Q2. How long has your company been in operation?**  
Vectrix was incorporated in March 2025. The product has been in production since September 2025 (public beta launch). We are a seed-stage company with $2.4M raised from Threshold Ventures.

**Q3. Provide your most recent SOC 2 report or equivalent certification.**  
Vectrix holds SOC 2 Type I certification, issued October 2025 via Vanta. We have committed to completing SOC 2 Type II by June 30, 2026. The Type I report is available under NDA upon request.

> **[COMMENT — Rachel Simmons, Jan 24]: Q3 — Type I is noted. We require Type II for production vendor approval per our HIPAA compliance posture. We understand you've committed to Type II by June 30 — we are provisionally accepting this with the right to terminate if Type II is not completed by Sept 30, 2026. This needs to be reflected in the SOW termination clause.**

**Q4. Do you have a dedicated security team?**  
At our current stage (10 employees), security is led by Marcus Webb (CTO) with operational security handled by Jordan Okafor (DevOps). We do not have a dedicated security hire at this time. Security practices are enforced through Vanta's continuous monitoring platform.

---

## SECTION B: Data Handling & Storage

**Q12. Where is customer data stored?**  
All Vectrix customer data is stored in AWS us-east-1 and us-west-2 regions (United States only). We do not store any customer data outside the United States. This includes our primary database (PostgreSQL on Neon, us-east-1), analytics database (ClickHouse Cloud, us-east-1), and all backups (S3, cross-region us-east-1 and us-west-2).

**Q13. What data does Vectrix store about our API traffic?**  
Vectrix stores API event metadata only: HTTP method, endpoint path, status code, response latency (ms), organization ID, and timestamp. We do not store request or response bodies, query parameters, authentication headers, or any payload data. We do not store PII from API calls.

**Q14. What is your data retention policy?**  
Standard: 90 days raw event data, 2 years hourly aggregates. For Meridian specifically: per our draft SOW, we will retain raw event data for 12 months per your requirement. This is configured as a per-organization retention setting (currently in development — VEC-47, targeted March 1, 2026).

**Q15. How is data encrypted at rest?**  
AES-256 via AWS-managed encryption keys for S3, EBS volumes, and ClickHouse Cloud. PostgreSQL data encrypted via Neon's managed encryption. All encryption keys are managed by the respective cloud providers with access logging enabled.

**Q16. How is data encrypted in transit?**  
TLS 1.2 minimum for all connections. TLS 1.3 supported and preferred. All internal service-to-service communication uses TLS.

---

## SECTION C: Access Control

**Q23. How do you manage employee access to production data?**  
Production database access is restricted to Marcus Webb (CTO) and Jordan Okafor (DevOps). Access requires MFA and SSH key authentication. All production access is logged via AWS CloudTrail. We do not have shared credentials.

**Q24. Do you conduct background checks on employees with data access?**  
Yes. All full-time employees undergo background checks via Checkr before their first day.

**Q31. Do you support SSO for customer authentication?**  
Yes. Vectrix uses Clerk as our auth provider, which supports SAML 2.0 and OIDC SSO. We will configure SAML SSO for Meridian's organization within 30 days of contract execution. Your IdP (we assume Okta or Azure AD) will be connected via SAML metadata exchange.

---

## SECTION D: Incident Response

**Q38. Describe your incident response process.**  
Vectrix uses PagerDuty for on-call alerting and Grafana Cloud for monitoring. Incidents are classified as P1 (service down, 15-minute acknowledge SLA), P2 (degraded performance, 1-hour acknowledge), or P3 (non-critical). Post-mortems are required for all P1 and P2 incidents, published within 48 hours. Our Dec 23, 2025 P1 incident (rate limiter failure, 90-minute downtime) is our most recent example; post-mortem available on request.

**Q39. How do you notify customers of security incidents?**  
We notify affected customers within 72 hours of discovering a security incident via email to the account owner. For incidents involving potential data exposure, we also follow applicable breach notification requirements.

---

## SECTION E: SLA & Reliability

**Q47. What SLA credits do you offer?**  
Current standard: 5% of monthly fee per 0.1% below 99.9% SLA target, up to a maximum credit of 30% of monthly fee per calendar month.

> **[COMMENT — Rachel Simmons, Jan 24]: Q47 — This is the item I flagged in my opening comment. The 30% cap does not meet our vendor standards for PHI-adjacent infrastructure (even though Vectrix only stores metadata, our security team classifies all API infrastructure as PHI-adjacent given that our APIs are part of our clinical data pipeline). We need to see this raised to 100% or uncapped before we can approve. Please confirm Vectrix's position on this before our legal team reviews the SOW.**

**Q48. What is your uptime track record?**  
Since public beta (September 15, 2025 – February 28, 2026): 99.71% uptime. Two incidents: Dec 23, 2025 (P1, ~90 min, rate limiter) and Feb 20, 2026 (P2, ~45 min, ClickHouse Cloud maintenance overlap). Both post-mortems available.

---

## SECTION F: Subprocessors

**Q61. List your material subprocessors.**

| Vendor              | Service                      | Data Processed                    | Location                  |
| ------------------- | ---------------------------- | --------------------------------- | ------------------------- |
| Amazon Web Services | Compute, storage, networking | All customer data                 | US (us-east-1, us-west-2) |
| Neon                | PostgreSQL database hosting  | Account data, config              | US (us-east-1)            |
| ClickHouse Cloud    | Analytics database           | API event metadata                | US (us-east-1)            |
| Confluent Cloud     | Kafka message queue          | API events in transit             | US (us-east-1)            |
| Upstash             | Redis cache                  | Session data, rate limit counters | US                        |
| Clerk               | Authentication               | User identity, org membership     | US                        |
| Typesense Cloud     | Search index                 | Endpoint names, org config        | US                        |
| Vanta               | Security compliance          | Compliance evidence               | US                        |
| PagerDuty           | Incident alerting            | Alert metadata                    | US                        |
| Grafana Cloud       | Monitoring                   | Infrastructure metrics            | US                        |

> **[COMMENT — Rachel Simmons, Jan 24]: Q61 — ClickHouse Cloud as a subprocessor needs more detail. Who is the underlying cloud provider for ClickHouse Cloud? Are they AWS, GCP, or Azure? We need confirmation that all ClickHouse infrastructure is on AWS us-east-1 specifically (not just "US"). Please update this response.**

---

## SECTION G: Open Items (as of Feb 3, 2026)

_Internal tracking — not visible to Rachel Simmons_

| Question                             | Status                                     | Owner               | Due       |
| ------------------------------------ | ------------------------------------------ | ------------------- | --------- |
| Q47 — SLA credit cap                 | In negotiation (Orion Legal)               | Priya + Orion Legal | Feb 24    |
| Q48 — ClickHouse subprocessor detail | Need to confirm AWS as underlying provider | Jordan              | Feb 10    |
| Q3 — SOC 2 Type II timeline          | Accepted with contractual backstop         | Priya               | Confirmed |
| Remaining questions (Q62–Q147)       | In progress                                | Nina                | Feb 14    |

---

_This document is shared externally with Meridian Health Systems. Do not include internal notes in responses to Rachel Simmons._  
_Internal contact: Nina Kowalski (nina@vectrix.io)_
