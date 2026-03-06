# Auth Provider Comparison: Firebase vs Clerk vs Auth0

**Author:** Marcus Webb (marcus@vectrix.io)  
**Date:** December 12, 2025  
**Decision:** Migrate to **Clerk**  
**Status:** Decision made — migration in progress (VEC-52, VEC-53)

---

## Context

We need org-level permissions and SAML SSO to close the Meridian deal ($2,800/mo, 85-engineer team with 200+ APIs). Firebase Auth — our current provider — has neither. This doc evaluates Firebase (status quo), Clerk, and Auth0.

---

## Requirements

| Requirement            | Priority | Notes                                                  |
| ---------------------- | -------- | ------------------------------------------------------ |
| Org-level permissions  | P0       | Multi-org model: users belong to orgs, orgs have roles |
| SAML SSO               | P0       | Required by Meridian Health Systems                    |
| React SDK quality      | High     | Sofia needs clean hooks                                |
| Backend JWT validation | High     | Go middleware needs to verify tokens                   |
| Webhook support        | Medium   | Sync user state to our DB                              |
| Pricing fit            | Medium   | We're pre-revenue-profitable; cost matters             |
| Migration complexity   | Medium   | We have ~14 paying customers on Firebase               |

---

## Option 1 — Firebase Auth (Status Quo)

**Cost:** Free tier (sufficient at our scale)

### Pros

- Already implemented; no migration cost
- Battle-tested, Google-backed reliability
- Works fine for basic email/password + OAuth

### Cons

- **No org-level permissions model.** Firebase users are flat. We've been faking org membership with custom claims, which is fragile.
- **No SAML SSO.** Google Identity Platform supports SAML but requires upgrading to the paid plan and significant custom implementation. Not a clean path.
- **Poor Go SDK.** Firebase Admin SDK for Go is functional but thin.
- **No webhook system.** Syncing user state to our PostgreSQL requires polling or custom triggers.

**Verdict:** Dead end for enterprise. Can't close Meridian with Firebase.

---

## Option 2 — Clerk

**Cost:** $49/month (current plan) — scales with MAU, reasonable

### Pros

- **First-class org model.** Clerk was built for B2B SaaS. `useOrganization()` hook works exactly as we need.
- **SAML SSO built in.** OIDC and SAML supported natively, including per-org SSO configuration.
- **React SDK is excellent.** `useAuth()`, `useUser()`, `useOrganization()` — Sofia confirmed these are clean and well-documented.
- **Go backend integration:** JWT verification via standard RS256; Clerk's JWKS endpoint makes this straightforward.
- **Webhooks:** Svix-powered webhook system; easy to keep our PostgreSQL user table in sync.
- **Migration path:** Clerk supports token migration — we can move users gradually without forcing re-auth.

### Cons

- New vendor dependency ($49/mo)
- Need to migrate 14 existing customers from Firebase tokens — some with non-standard token formats (Brainloop is a known case)
- Limited track record at very high scale (we're not at that scale, but still)

**Verdict: Recommended.** Best fit for our B2B model. Unblocks Meridian.

---

## Option 3 — Auth0

**Cost:** ~$240/month at our user count (Essentials plan)

### Pros

- Mature, enterprise-grade
- SAML SSO, org model, all the features
- Battle-tested at massive scale

### Cons

- **Expensive.** $240/mo vs Clerk's $49/mo for equivalent features. 5x cost for the same functional outcome.
- **More complex than we need.** Auth0's configuration surface is large; our team doesn't have Auth0 expertise.
- **React SDK is heavier.** More boilerplate than Clerk's hooks.
- **Overkill at our scale.** Auth0 is designed for 50K+ MAU enterprise scenarios. We have ~40 users.

**Verdict:** Works, but too expensive and complex for what we need right now. Revisit at Series A if we need the enterprise features Auth0 does better than Clerk.

---

## Decision

**Migrate to Clerk.**

- Unblocks Meridian's SAML SSO requirement (P0)
- Best React developer experience (Sofia's input validated this)
- Best Go backend integration path (Liam reviewed)
- $49/mo is acceptable
- Token migration strategy is sound

**Migration plan:**

- Sofia handles frontend: replace Firebase Auth hooks with Clerk equivalents, dual-token support during migration window
- Liam handles backend: JWT middleware accepts both Firebase and Clerk tokens during transition; fallback removed after all customers migrated
- Target completion: January 31, 2026

Issues: VEC-52 (SSO/Clerk backend), VEC-53 (frontend auth context)

---

_-M_  
_December 12, 2025_
