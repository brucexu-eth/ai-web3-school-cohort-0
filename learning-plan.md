# Personal Learning Plan

Based on the [AI × Web3 School Handbook](https://aiweb3.school/zh/handbook/) and WCB course page.

Last updated: 2026-05-18

## Operating principles

- **Minimum viable learning**: one concept + one concrete artifact per day.
- **Proof-of-work**: every learning session leaves a public note, experiment, feedback item, or proposal fragment.
- **Safety-first Web3 Agent design**: read/write separation, deterministic tool boundaries, simulation, human confirmation, trace, and regression cases.
- **No secret leakage**: WCB API keys, wallet keys, internal links, and private information stay out of this public repo.

## Phase 0 — Setup and baseline

| Item | Status | Evidence |
|---|---|---|
| Create public GitHub learning repo | Done | https://github.com/brucexu-eth/ai-web3-school-cohort-0 |
| Initialize repo structure | Done | `README.md`, `profile.md`, `daily/`, `tasks/`, `experiments/`, `handbook-feedback/`, templates |
| Create Day 1 daily note | Done | `daily/2026-05-17.md` |
| Create feedback workflow | Done | `handbook-feedback/README.md` and `handbook-feedback/TEMPLATE.md` |

## Phase 1 — Bridge foundations, high priority

| Order | Topic | Handbook | Output artifact | Status |
|---|---|---|---|---|
| 1 | Agent | https://aiweb3.school/zh/handbook/ai/agent/ | Agent execution-loop summary | Done |
| 2 | Chain-aware Context | https://aiweb3.school/zh/handbook/bridge/chain-aware-context/ | Context package checklist | Done |
| 3 | Web3 Tool Use | https://aiweb3.school/zh/handbook/bridge/web3-tool-use/ | Tool schema + permission matrix | In progress |
| 4 | Agent Workflow | https://aiweb3.school/zh/handbook/bridge/agent-workflow/ | Task graph + state machine draft | In progress |
| 5 | Agent Wallet | https://aiweb3.school/zh/handbook/bridge/agent-wallet/ | Wallet permission model | Next |
| 6 | Machine Payment | https://aiweb3.school/zh/handbook/bridge/machine-payment/ | Payment flow notes | Next |
| 7 | Settlement & Escrow | https://aiweb3.school/zh/handbook/bridge/settlement-and-escrow/ | Settlement risk checklist | Next |

## Phase 2 — Risk, verification, and identity

| Order | Topic | Handbook | Output artifact | Status |
|---|---|---|---|---|
| 8 | Agent Identity | https://aiweb3.school/zh/handbook/bridge/agent-identity/ | Identity / accountability notes | Pending |
| 9 | Agent Trust & Reputation | https://aiweb3.school/zh/handbook/bridge/agent-trust-and-reputation/ | Reputation signals map | Pending |
| 10 | Verifiable AI | https://aiweb3.school/zh/handbook/bridge/verifiable-ai/ | Verification boundary notes | Pending |
| 11 | AI Security | https://aiweb3.school/zh/handbook/bridge/ai-security/ | Threat model + mitigations | Pending |
| 12 | AI Privacy | https://aiweb3.school/zh/handbook/bridge/ai-privacy/ | Privacy boundary checklist | Pending |
| 13 | Governance AI | https://aiweb3.school/zh/handbook/bridge/governance-ai/ | DAO workflow use case | Pending |

## Phase 3 — Project / Hackathon direction

Selected direction: **Private Agentic Checkout**.

Thesis: personal Agents will buy goods for users, but Agentic Commerce needs a privacy and permission layer before payment execution. The project demonstrates an ETH-denominated electronics purchase flow where Hermes prepares the checkout, Bitrefill / LI.FI / a mock route plans payment, a privacy policy minimizes personal-data disclosure, and an audit receipt records what was authorized.

Implementation lead to evaluate: **Bitrefill** may be a practical route for the demo if it supports a relevant electronics-related gift card / voucher / invoice flow with acceptable privacy and payment boundaries.

Primary track fit:

1. **Agentic Commerce / Payment** — Agent prepares a purchase and crypto payment route.
2. **AI Security / Privacy** — privacy layer controls what personal data, wallet identity, and payment context leave the user’s local Agent.

Demo scenario:

```text
Hermes, buy me an electronics item under 0.02 ETH. Use ETH if possible. Do not expose my main wallet, real email, or unnecessary personal information.
```

Final deliverables:

- product / protocol spec;
- GitHub repo with schemas and examples;
- small working prototype or mocked end-to-end flow;
- demo video script;
- research-style report on privacy risks in Agentic Commerce.

Project docs:

- `docs/private-agentic-checkout-brief.md`
- `docs/private-agentic-checkout-daily-plan.md`

Decision rule: keep the scope narrow enough for solo work. Do not build full e-commerce, full anonymity, or automatic wallet spending. Prove the core insight: **Agentic checkout approval must cover purchase intent, payment route, privacy disclosure, and audit receipt together.**

## Daily cadence

- Morning / start session: pick today's Handbook chapter and one artifact.
- During session: separate facts, assumptions, open questions, and feedback.
- End session: update daily note, generate check-in draft, manually submit to WCB, paste submission link back.
- Weekly: summarize artifacts and select/refine project direction.
