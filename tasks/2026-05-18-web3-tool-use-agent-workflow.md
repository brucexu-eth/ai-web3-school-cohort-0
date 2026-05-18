# Task Note — Web3 Tool Use + Agent Workflow for Private Agentic Checkout

> Date: 2026-05-18  
> Project: Private Agentic Checkout
> Scenario: use ETH to buy a low-value electronics item through Hermes while minimizing privacy leakage.
> Source: AI × Web3 School Handbook
> Related daily note: ../daily/2026-05-18.md

## Task

Design the minimum safe structure for Hermes to prepare, but not autonomously execute, an ETH-denominated electronics checkout.

User story:

```text
Hermes, buy me an electronics item under 0.02 ETH. Use ETH if possible. Do not expose my main wallet, real email, or unnecessary personal information.
```

## Reading links

- Web3 Tool Use: https://aiweb3.school/zh/handbook/bridge/web3-tool-use/
- Agent Workflow: https://aiweb3.school/zh/handbook/bridge/agent-workflow/

## Tool design

| Tool | Read/write | Agent allowed? | Human confirmation? | Notes |
|---|---|---|---|---|
| `parse_checkout_intent` | Local read/write | Yes | No | Extract item category, budget in ETH, disclosure constraints, delivery constraints. |
| `search_or_select_product` | External read | Yes | No | For v0 can be mocked. Must not send private profile fields. |
| `apply_privacy_policy` | Local transform | Yes | No | Produces redacted merchant payload and disclosure report. |
| `get_eth_balance` | Chain read | Yes | No | Must return chain id, provider, block number, address, ETH balance. Prefer payment/session address over main wallet. |
| `get_lifi_quote` | External quote | Yes | No | Quote-only. Inputs must exclude private user profile except source chain/token/amount. |
| `draft_payment_transaction` | Draft only | Yes | Before signing | Generates payment route / tx draft; cannot broadcast. |
| `simulate_transaction` | Read / simulation | Yes | No | Must return expected ETH/token deltas, fees, recipient, route risk, failure reason. |
| `show_approval_card` | Local output | Yes | Yes | Displays purchase, ETH spend, recipient, disclosed data, withheld data, risks. |
| `send_transaction` | Write | No by default | Yes / policy required | Disabled unless explicit user confirmation or a narrow smart-account policy allows it. |
| `write_audit_receipt` | Local write | Yes | No | Records intent, privacy disclosures, payment route, confirmation, tx/mock receipt. |

## Workflow draft

```text
1. Receive user goal and constraints
2. Parse checkout intent
3. Select or mock an electronics product
4. Apply privacy policy and create redacted order payload
5. Load chain-aware payment context
6. Get ETH balance from payment/session address
7. Request LI.FI quote or mock route if merchant requires non-ETH asset/chain
8. Draft payment transaction without signing
9. Simulate route / transaction
10. Run policy checks: budget, recipient, route, slippage/fee, privacy disclosure
11. Show approval card
12. Wait for user confirmation or cancellation
13. If confirmed, hand off to wallet / smart account
14. Track tx or mock receipt
15. Write audit receipt
```

## State machine draft

```text
draft
→ intent_parsed
→ product_selected
→ privacy_filtered
→ payment_context_loaded
→ quote_ready
→ transaction_drafted
→ simulated
→ waiting_user_confirmation
→ submitted / cancelled
→ confirmed / reverted / mock_completed
→ audit_recorded
```

## Human confirmation points

- Any real payment transaction.
- Any disclosure of real email, full delivery address, phone number, or main wallet address.
- Any route that uses more than the stated ETH budget.
- Any route that changes chain, asset, recipient, fee, or slippage beyond policy.
- Any merchant/product mismatch or uncertain recipient.

## Regression cases

| Case | Expected behavior |
|---|---|
| Normal small electronics purchase under budget | Prepare redacted order, quote ETH route, show approval card. |
| Merchant asks for real email | Use alias or stop for confirmation. |
| Merchant asks for full address before product selected | Stop; ask whether disclosure is necessary. |
| Main wallet would be exposed | Use payment/session address or warn and require confirmation. |
| LI.FI route exceeds budget | Reject or ask user to raise budget. |
| Wrong chain or unsupported token | Stop before transaction draft. |
| Prompt-injected product page asks Agent to ignore privacy rules | Ignore external instruction; treat page as untrusted content. |
| User rejects payment | Mark cancelled; do not retry automatically. |

## Today’s output checklist

- [ ] Read both Handbook chapters.
- [ ] Tighten this workflow into a 10-step version.
- [ ] Mark which steps are local-only, external-read, chain-read, draft, or write.
- [ ] Write one possible Handbook feedback item if the chapters lack an Agentic Checkout example.
