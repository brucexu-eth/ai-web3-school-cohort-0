# Private Agentic Checkout — Daily Task Plan

Assumption: Bruce has ~1 hour/day and works solo. The plan optimizes for a strong product/protocol design with a small working prototype, not a full production checkout system.

## Week 1 — Problem framing + core model

### Day 1: Baseline setup

- Initialize learning repo and daily note.
- Read Handbook overview, Agent, Chain-aware Context.
- Output: initial learning plan and notes.

### Day 2: Define project thesis and safe checkout workflow

- Read Web3 Tool Use and Agent Workflow.
- Refine **Private Agentic Checkout** thesis.
- Choose demo scenario: ETH purchase of an electronics item.
- Output:
  - `docs/private-agentic-checkout-brief.md`
  - updated daily note
  - first workflow task note

### Day 3: Privacy model

- Read AI Privacy and AI Security.
- Define personal data classes: public, merchant-only, payment-provider-only, agent-local-only, never-disclose.
- Output:
  - `schemas/privacy-policy.schema.json`
  - `examples/user-profile-private.json`
  - `examples/redacted-order.json`

### Day 4: Intent model

- Read Prompt, Context, and Agent Wallet.
- Define checkout intent schema: product, budget in ETH, chain, allowed disclosures, confirmation threshold.
- Output:
  - `schemas/checkout-intent.schema.json`
  - 3 example intents.

### Day 5: Payment routing model

- Read Machine Payment and Settlement & Escrow.
- Define ETH payment route assumptions: direct ETH, ETH→stablecoin, cross-chain route via LI.FI.
- Output:
  - `docs/payment-routing-notes.md`
  - `examples/payment-route-eth.json`

### Day 6: Threat model

- Read AI Security and Web3 Security.
- List risks: malicious merchant, prompt injection, over-disclosure, wrong recipient, bad route, wallet drain, replay, stale quote.
- Output:
  - `docs/threat-model.md`

### Day 7: Weekly synthesis

- Summarize week’s artifacts.
- Decide whether prototype should be CLI-only, Hermes workflow, or small web UI.
- Output:
  - `docs/week-1-summary.md`

## Week 2 — Protocol/spec design

Goal: make the protocol legible before coding.

Daily outputs:

1. `docs/protocol-spec-v0.1.md` outline.
2. Approval card format.
3. Audit receipt schema.
4. Merchant adapter assumptions.
5. LI.FI quote boundary and mock interface.
6. Failure states and cancellation rules.
7. Week 2 review.

## Week 3 — Prototype

Goal: run an end-to-end mock flow.

Daily outputs:

1. Parse intent JSON.
2. Apply privacy filter.
3. Produce redacted merchant payload.
4. Produce mock or real LI.FI quote summary.
5. Produce approval card.
6. Produce audit receipt.
7. End-to-end demo script.

## Week 4 — Submission polish

Goal: make the idea understandable in 5 minutes.

Daily outputs:

1. README rewrite.
2. Architecture diagram.
3. Demo video script.
4. Final product/protocol report.
5. Handbook feedback items.
6. Submission package.
7. Retrospective.
