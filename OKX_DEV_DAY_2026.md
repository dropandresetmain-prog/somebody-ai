# Somebody × OKX Dev Day 2026 — Project SSOT

Status: ACTIVE
Date: 17 September 2026
Implementation branch: `hackathon/okx-dev-day-2026`
Pre-OKX baseline SHA: `709a169a1a4f71b8dc2d7427438ff514999fb07e`
Official build window: 17–25 September 2026
Submission deadline: 25 September 2026, 23:59 UTC

This document is the project-level source of truth for the OKX Dev Day extension of Somebody. Existing `PROJECT_BRIEF.md` and `ARCHITECTURE.md` remain authoritative for the pre-existing Somebody product and architecture unless this file explicitly adds an OKX-specific decision.

## 1. Product thesis

### Broader Somebody vision

One person should be able to operate with the functional reach of a much larger company.

Somebody is the AI manager for that company. The user delegates an outcome; Somebody owns the work and marshals the resources required to get it done.

Initial practical users remain one-person companies, founder-led businesses and lean SMEs where founders, GMs or operations leads have more work than specialist headcount.

Core problem:

> Small teams do not lack things to do. They lack owners, capacity and access to some expertise or resources.

### OKX project thesis

The OKX Dev Day project proves one missing capability:

> Somebody can decide whether a required business capability should be MADE internally or BOUGHT externally, then assemble those resources to complete the founder's objective.

The intellectual centre of the project is Make vs Buy. OKX AI is the external procurement/economic layer, not the product itself.

### Web3 boundary

Somebody is not being converted into a Web3 application.

Normal internal orchestration, state, reasoning and company tools remain conventional software.

Web3 is useful at the cross-company machine-commerce boundary: an AI manager can discover or call an external provider, understand its price, pay without establishing a traditional billing/API-key relationship, receive a result and retain a transaction/receipt trail.

Marketing shorthand may be "Web3 infrastructure for people who do not care about Web3", but this is not the primary product pitch.

## 2. Make vs Buy policy v1

The model may propose capabilities and required resources. Application policy owns the sourcing decision. The model cannot override it.

### MAKE by default

A capability is MAKE when it can reasonably be constructed from resources the company already controls, including:

- generic LLM reasoning;
- public web information;
- company-owned data and records;
- existing authenticated company tools;
- ordinary compute;
- reusable internal tooling.

Important invariant:

> "We do not currently have an agent for it" is never, by itself, a reason to buy.

If a worker can be created around resources already owned by the company, the capability is internal.

### BUY

A capability becomes a BUY candidate when it depends on a genuinely external/scarce resource, for example:

- proprietary or licensed data;
- privileged platform access;
- credentials the company does not possess;
- independently controlled real-world action;
- independent attestation or verification;
- specialist infrastructure or compute that is uneconomic to reproduce;
- an external network, identity or other resource controlled by another economic entity.

Guiding principle:

> Never buy generic cognition merely because another agent wrapped an LLM. Buy scarce capability or resources.

### BLOCKED

If the capability requires an external resource and no approved provider can supply it within policy/budget, Somebody must block/escalate rather than fabricate success.

## 3. Scope of the hackathon build

The canonical end-to-end proof must contain all of the following:

1. Founder gives Somebody one business outcome.
2. Somebody identifies bounded required capabilities.
3. At least one capability is classified MAKE and actually executed internally.
4. At least one capability is classified BUY because it requires a genuinely external/scarce resource.
5. A bounded spend is explicitly authorised according to policy.
6. Somebody performs a real OKX AI / x402 external purchase.
7. The external result and payment receipt are persisted.
8. Somebody verifies the external delivery before treating it as complete.
9. Internal and external outputs are combined into one useful business outcome.
10. The UI makes the MAKE / BUY reasoning and execution trace understandable in a 2–4 minute demo.

## 4. Implementation base and reuse boundary

### Base repository

The OKX project continues in `dropandresetmain-prog/somebody-ai`.

Do not create a separate implementation product unless a concrete technical blocker forces it. The branch above starts from the exact pre-OKX baseline so the judged delta remains explicit.

### Reuse from Somebody

Reuse or adapt existing working patterns rather than rebuilding them:

- `lib/reliability/core.ts`: transitions, approval/authority gate, receipt verification pattern and completion assertions;
- model-proposes / application-enforces / external-evidence-proves architecture;
- deterministic idempotency/effect identity patterns from procurement;
- existing agent Runner/provider scaffolding where appropriate;
- Convex realtime state and event-log patterns;
- existing approval and proof UI primitives where useful;
- existing write-boundary/local-control protections;
- the current test harness as regression evidence.

Do not broadly refactor the existing procurement mission. Its quote/vendor/RFQ/purchase-order aggregate is specific to the prior demo and currently works.

The OKX work should be a bounded sibling flow rather than a rewrite of the existing procurement workflow.

### Reuse from Army of Interns

`army-of-interns` is an R&D source, not a product dependency and not a brand requirement.

Current audit indicates that the useful contribution is mostly patterns, not code:

- controlled capability vocabulary / validation;
- deny-by-default tool-permission envelope.

Do not wholesale import Army's schema, runtime, Telegram/Twilio integrations, org chart, personalities, promotions, ranks, demo state machines or worker-to-worker complexity.

No requirement exists to preserve the name "Army of Interns" in the OKX project.

## 5. Demo status

### Current lead: Candidate A — supplier invoice verification

Founder objective:

> "Our supplier sent invoice INV-4471 for S$4,800 and says their bank details changed. Pay it if it is legitimate."

Potential MAKE work:

- extract invoice fields;
- match invoice to existing PO/vendor records;
- detect changed bank/payment details or contact discrepancies;
- draft an evidence memo and supplier response.

Potential BUY:

- independently controlled out-of-band verification of the supplier via OKX AI provider `Dial` (#9753), currently reported to perform a real phone call and return signed evidence.

Potential outcome:

- HOLD / RELEASE-eligible / ESCALATE recommendation with evidence and audit trail.

### Status: NOT YET CANONICAL

The founder has not accepted this demo as final.

It becomes canonical only after today's provider/payment gate proves that the BUY is both technically reliable and conceptually legitimate.

Required gate:

1. Dial can reach a consenting test number we control.
2. The official OKX payment path can purchase the service programmatically.
3. The result/proof envelope can be independently verified.
4. Payment state/receipt can be captured without ambiguous double-spend behaviour.
5. The BUY case survives the Make-vs-Buy challenge: the value must be an external real-world verification/attestation capability, not merely "we did not integrate telephony ourselves".

If this gate fails, choose another demo before building demo-specific UI. A current fallback family is OKX/OKLink external payee-risk data, but that is not accepted as canonical either.

## 6. Current critical path

### M0 — External capability/payment gate

Prove one paid OKX AI service call end to end from code with a consenting test target. Capture payment terms, transaction/receipt, provider response and verification evidence.

This is the first implementation milestone because it resolves the highest-risk dependency.

### M1 — Capability sourcing policy

Implement a small deterministic policy surface that supports at least:

- owned resources -> MAKE;
- external scarce resource with approved provider -> BUY;
- unavailable external resource -> BLOCKED;
- generic LLM/public-web wrapper -> remain MAKE/reject external purchase.

### M2 — Minimal internal execution

Implement only enough dynamic internal capability execution to prove the MAKE branch for the canonical demo. Do not build a generic multi-agent company runtime.

### M3 — Verified BUY lifecycle

Authorise spend -> attempt purchase -> persist receipt/result -> verify -> mark complete.

Retries or crashes must not create silent duplicate payments. Attempt/submission/verification states must remain distinguishable.

### M4 — One end-to-end business outcome

Combine the MAKE and BUY results into a useful founder-facing result.

### M5 — Demo surface and hardening

Expose the reasoning and execution trace clearly, then harden the one path. Record a complete demo as soon as the end-to-end flow is reliable.

### M6 — Submission

Document the build-period delta, exact candidate SHA, integration URL/listing as required, evidence, setup and 2–4 minute demo.

## 7. Hard non-goals

Do not add unless required to rescue the accepted demo:

- generic autonomous-company architecture;
- arbitrary org charts or departments;
- worker personalities, promotions or ranks;
- worker-to-worker conversations;
- multiple polished workflows;
- multiple external vendors on the critical path;
- vendor auctions;
- generic marketplace indexing;
- A2A negotiation/escrow;
- a universal payment abstraction across Stripe/OKX/etc.;
- a wholesale rewrite of Somebody's current procurement domain;
- speculative infrastructure;
- token/trading gimmicks.

Reliable end-to-end execution wins over breadth.

## 8. Current risks / unresolved decisions

Act Now:

- Validate the current lead external provider/payment path before building around it.
- Confirm whether buyer-side OKX AI integration is sufficient for the Build a Company track or whether a published seller listing is also required.

Investigate Now:

- Exact unattended/programmatic contract of the OKX buyer tooling.
- Whether to reuse the existing Convex development deployment or create a separate OKX development deployment.
- Exact provider proof/receipt format for the accepted demo.

Park for Later:

- Publishing Somebody itself as a paid marketplace provider, unless track interpretation makes this mandatory.
- Broader marketplace discovery and provider competition.
- Generalized economic optimization between internal and external execution.

Ignore / Accept Risk:

- Army of Interns' unfinished broader organization simulation. It is not required for this milestone.

## 9. Completion condition

The OKX hackathon project is complete when a single reliable demo shows:

> one founder -> one objective -> one internal capability made -> one genuinely scarce external capability bought through OKX -> external delivery verified -> one useful completed business outcome.

Nothing beyond this is required for the core submission.