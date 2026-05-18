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

Choose one concrete prototype direction after the Bridge pass:

1. **Read-only DAO proposal research Agent** — lower asset risk, strong governance fit.
2. **Chain-aware transaction explainer** — strong context/citation discipline, good demo surface.
3. **Agent wallet permission simulator** — directly tests session keys, policy, human-in-the-loop, and trace.
4. **Web3 Agent eval harness** — high leverage dev tooling: regression cases for wrong-chain, infinite approve, stale oracle, prompt injection, user rejection.

Decision rule: pick the direction with the clearest demo, safest scope, and best proof-of-work within one week.

## Daily cadence

- Morning / start session: pick today's Handbook chapter and one artifact.
- During session: separate facts, assumptions, open questions, and feedback.
- End session: update daily note, generate check-in draft, manually submit to WCB, paste submission link back.
- Weekly: summarize artifacts and select/refine project direction.
