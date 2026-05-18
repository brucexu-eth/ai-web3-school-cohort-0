# Task Note — Web3 Tool Use + Agent Workflow

> Date: 2026-05-18  
> Source: AI × Web3 School Handbook  
> Related daily note: ../daily/2026-05-18.md

## Task

Design the minimum safe structure for an AI Agent that can prepare, but not autonomously execute, a small ERC-20 swap.

## Reading links

- Web3 Tool Use: https://aiweb3.school/zh/handbook/bridge/web3-tool-use/
- Agent Workflow: https://aiweb3.school/zh/handbook/bridge/agent-workflow/

## Tool design

| Tool | Read/write | Agent allowed? | Human confirmation? | Notes |
|---|---|---|---|---|
| `get_eth_balance` | Read | Yes | No | Must return chain id, provider, block number, address, balance. |
| `read_erc20_allowance` | Read | Yes | No | Must verify network, token, owner, spender, ABI. |
| `draft_erc20_approve` | Draft only | Yes | Before signing | Generates calldata only; cannot broadcast. |
| `simulate_transaction` | Read / simulation | Yes | No | Must return expected token/value deltas and failure reason. |
| `send_transaction` | Write | No by default | Yes / policy required | Disabled unless explicit user confirmation or smart-account policy allows it. |

## Workflow draft

```text
1. Receive user goal and constraints
2. Load chain-aware context
3. Read balance and allowance
4. Query price / liquidity / route
5. Draft candidate transaction
6. Simulate candidate transaction
7. Run policy checks
8. Show human-readable risk summary
9. Wait for user confirmation or cancellation
10. If confirmed, hand off to wallet / smart account
11. Track receipt and write trace
```

## State machine draft

```text
draft
→ context_loaded
→ plan_ready
→ transaction_drafted
→ simulated
→ waiting_user_confirmation
→ submitted / cancelled
→ confirmed / reverted
→ recorded
```

## Regression cases

| Case | Expected behavior |
|---|---|
| Normal small swap | Prepare draft, simulate, ask for confirmation. |
| Wrong chain | Stop and ask user to switch / confirm chain. |
| Slippage too high | Reject or require explicit adjustment. |
| Insufficient balance | Stop before draft or before simulation. |
| User rejects signature | Mark cancelled; do not retry automatically. |
| Infinite approve request | Reject unless explicit policy allows it. |
| Prompt-injected token docs | Ignore instructions from untrusted docs; use docs only as context, not authority. |

## Open questions

- What should be the canonical Tool Log schema across all AI × Web3 examples?
- Which parts should be represented as code, JSON schema, or a diagram for the Hackathon phase?
