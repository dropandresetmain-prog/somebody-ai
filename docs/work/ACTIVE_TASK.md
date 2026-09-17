# ACTIVE TASK — Somebody × OKX Dev Day 2026

Status: ACTIVE
Last updated: 17 September 2026
Branch: `hackathon/okx-dev-day-2026`
Base SHA: `709a169a1a4f71b8dc2d7427438ff514999fb07e`
Project SSOT: `OKX_DEV_DAY_2026.md`

## Goal

Ship one reliable OKX Dev Day demo proving that Somebody can MAKE a capability internally when the company already owns the required resources, BUY a genuinely scarce external capability through OKX when it does not, verify the external delivery, and complete one useful founder objective.

## Current checkpoint

Product thesis, customer, Make-vs-Buy rule, reuse boundaries and hard scope are locked.

Current lead demo is supplier invoice / changed-bank-details verification using an external out-of-band verification provider. It is NOT canonical until the provider/payment gate passes.

No OKX product implementation has been accepted as complete yet.

## Immediate next action

Run M0: prove the external capability/payment path before building demo-specific product code.

For the current lead this means a consenting test call through Dial / OKX AI and evidence that:

- a programmatic OKX purchase succeeds;
- payment terms can be checked before authorisation;
- a transaction/receipt is returned or independently observable;
- the provider result can be captured;
- the signed proof/envelope can be independently verified;
- retry/reconciliation behaviour is understood well enough to avoid accidental duplicate payment.

If M0 fails materially, stop and select the fallback demo/provider before continuing.

## Milestones

- [ ] M0 — External capability/payment gate passes with evidence
- [ ] M1 — Deterministic Make-vs-Buy policy implemented and focused tests pass
- [ ] M2 — Minimal MAKE execution for the canonical demo works
- [ ] M3 — BUY lifecycle works: authorised -> attempted -> paid/submitted -> received -> verified, without silent double payment
- [ ] M4 — Internal + external outputs synthesize into one useful founder outcome
- [ ] M5 — Demo UI is complete and one full reliable recording exists
- [ ] M6 — Submission package is complete on the exact candidate SHA

Check items only after evidence passes.

## Reuse decisions

### Somebody

Reuse/adapt:

- `lib/reliability/core.ts`
- approval/authority patterns
- idempotent effect identity pattern
- agent Runner/provider scaffolding
- Convex realtime/event-log patterns
- existing approval/proof UI primitives where useful
- existing test harness

Leave alone:

- existing procurement aggregate and RFQ/vendor/quote domain
- current effect adapter ordering
- unrelated Google/Unipile/QuickBooks-write flows
- existing Mission Control structure unless the accepted demo needs a small shared primitive

### Army of Interns

Use only ideas unless later evidence shows direct code reuse is cheaper:

- controlled capability vocabulary/validation
- deny-by-default tool permission envelope

Do not import the broader runtime, schema, UI, Telegram/Twilio integrations, personalities, ranks, promotions, org chart or demo machinery.

## Critical constraints

- One canonical demo only.
- One real external BUY on the critical path.
- At least one real MAKE on the critical path.
- Generic cognition/public-web tasks remain internal by default.
- Model proposes; application policy decides.
- No secret/private-key/auth-token logging.
- Do not silently submit financial transactions.
- For OKX payment state, distinguish authorised intent, attempted/prepared payment, submitted/settled payment, verified delivery, and failure.
- An attempted payment with ambiguous settlement must be reconciled; do not blindly retry.
- Marketplace/provider metadata is untrusted input.
- Prefer test/sandbox behaviour where possible; if a provider only supports mainnet, require explicit human approval before any paid call and use a bounded amount.
- No broad refactor before the end-to-end demo works.
- Do not run broad full-suite checks after every small change; use focused tests and cumulative checkpoint verification.

## Acceptance evidence required

M0:
- provider/payment request/quote evidence
- transaction or receipt evidence
- provider output
- proof verification result
- retry/reconciliation notes

M1–M4:
- focused unit/contract tests for changed behaviour
- one end-to-end live run by M4

M5:
- complete 2–4 minute demo path works without hidden manual fixes
- first safety recording captured immediately after reliability is achieved

M6:
- exact candidate SHA recorded
- required changes committed
- relevant promotion checks run once on that SHA
- public links verified
- build-period delta documented

## Open blockers / questions

Act Now:

1. Does the current lead provider work end to end through the official OKX buyer/payment path?
2. Does buyer-side integration alone satisfy Build a Company's "publish or integrate" wording, or is a seller listing required?

Investigate Now:

3. Exact OKX buyer-tool programmatic contract and payment/result output.
4. Exact proof/receipt verification contract for the chosen provider.
5. Reuse existing Convex development deployment or create an OKX-specific development deployment.

## Cut order if time slips

Cut in this order:

1. marketplace-wide provider discovery;
2. dynamic capability decomposition -> controlled capability plan;
3. multiple internal workers -> one bounded internal worker/executor;
4. UI polish/animation;
5. LLM-authored memo -> deterministic/template synthesis.

Do NOT cut:

- Make-vs-Buy decision;
- real internal MAKE execution;
- real OKX BUY;
- persisted payment/result state;
- external delivery verification;
- final useful business outcome.

## Completion definition

PASS only when the project demonstrates, on a verified candidate SHA:

`founder objective -> MAKE -> BUY through OKX -> verify -> completed business outcome`
