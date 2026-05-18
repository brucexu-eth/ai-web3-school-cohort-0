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

## New implementation lead — Bitrefill

A friend recommended **Bitrefill** as a possible practical route for the checkout demo.

Why it may fit:

- It is closer to a real crypto checkout path than a fully mocked merchant.
- It may support electronics-related purchases indirectly through gift cards / vouchers.
- It lets the project focus on Agent workflow, payment preparation, user confirmation, privacy disclosure, and audit receipt instead of building a merchant network.

Open checks:

- Which Bitrefill products are relevant to the electronics scenario?
- Which countries / merchants are available for the target demo region?
- What payment assets, chains, expiry windows, and recipient formats are supported?
- Does the checkout leak email, IP, wallet address, order history, or other identifiable metadata?
- Can the demo stop at quote / invoice / draft stage without forcing a real purchase?

## Tool design

| Tool | Read/write | Agent allowed? | Human confirmation? | Notes |
|---|---|---|---|---|
| `parse_checkout_intent` | Local read/write | Yes | No | Extract product category, ETH budget, delivery constraints, disclosure constraints, and allowed payment route. |
| `search_or_select_product` | External read | Yes | No | For v0 can be mocked or Bitrefill-backed. Must not send private profile fields. |
| `apply_privacy_policy` | Local transform | Yes | No | Produces redacted merchant payload and disclosure report. |
| `write_tool_log` | Local write | Yes | No | Append every tool call, inputs, outputs, source, timestamp, policy result, error, chain id, block number, tx hash if any. |
| `check_tool_permissions` | Local policy gate | Yes | No for read; Yes before elevated permission | Central permission boundary. Blocks unsafe tool calls before execution. Dangerous because a bad policy can authorize unsafe actions. |
| `get_eth_balance` | Chain read | Yes | No | Query balance for payment/session address. Return chain id, provider, block number, address, and ETH balance. |
| `contract_read` | Chain read | Yes | No | Read token, allowance, recipient, invoice/payment contract state, or verified protocol data. Requires chain and contract verification. |
| `get_payment_quote` | External quote | Yes | No | Quote-only. Could be LI.FI, Bitrefill invoice quote, or mock provider. Inputs must exclude private user profile except required payment fields. |
| `draft_payment_transaction` | Draft only | Yes | Before signing/sending | Generates transaction draft / calldata / route; cannot broadcast. |
| `simulate_transaction` | Read / simulation | Yes | No | Return expected deltas, fees, recipient, route risk, failure reason, and whether policy passes. |
| `explorer_lookup` | External / chain read | Yes | No | Verify submitted tx status, confirmations, recipient, emitted events, and final status. Useful after wallet handoff. |
| `small_amount_whitelist_payment` | Constrained write | Only under explicit policy | Policy required; usually user-confirmed | Optional smart-account / session-key path for very small whitelisted payments. Must cap asset, amount, recipient, chain, expiry, and frequency. |
| `defi_route` | DeFi read/write planning | Quote/simulate yes; execute no by default | Yes for any route execution | Used for swap / bridge / token transfer route. High risk due slippage, approvals, bridges, MEV, and protocol risk. |
| `wallet_action` | Wallet write boundary | No by default | Yes | Separate connect / sign / send / approve / revoke. Agent can prepare handoff but should not silently sign. |
| `contract_write` | Chain write | No by default | Yes | Any contract mutation is high risk; requires explicit calldata display, simulation, policy pass, and user confirmation. |
| `write_audit_receipt` | Local write | Yes | No | Records intent, privacy disclosures, quote/route, confirmation, tx/mock receipt, and final status. |

## Dangerous tools / boundaries

Most dangerous:

1. `check_tool_permissions` — if this layer is wrong, unsafe tools become allowed.
2. `defi_route` — swap / bridge / token movement can lose funds through price, route, bridge, approval, or recipient mistakes.
3. `wallet_action` — signing/sending is the actual user authority boundary.
4. `contract_write` — arbitrary state mutation; must never be hidden behind natural language.

Medium risk:

- `draft_payment_transaction` because bad calldata can mislead the user if not decoded and simulated.
- `small_amount_whitelist_payment` because convenience policies can become spending loopholes.
- `explorer_lookup` is read-only but can create false confidence if chain, recipient, or confirmation depth is wrong.

## Workflow draft — 10-step checkout flow

```text
1. Receive user goal and constraints.
2. Parse checkout intent: product, budget, region, privacy constraints, ETH preference.
3. Select / mock product or Bitrefill purchase path.
4. Apply privacy policy and create redacted merchant/payment payload.
5. Load chain-aware payment context and check balance / allowance / relevant contract reads.
6. Get payment quote or route: direct ETH, Bitrefill invoice, LI.FI swap/bridge, or mock route.
7. Draft transaction and simulate route without signing or broadcasting.
8. Run policy checks: budget, recipient, chain, slippage/fee, expiry, privacy disclosure, tool permissions.
9. Show one approval card and wait for user confirmation or cancellation.
10. If confirmed, hand off to wallet / smart account, track via explorer, and write audit receipt.
```

## Human confirmation point

Current decision: **one explicit confirmation at payment time is enough for the checkout demo**, as long as the approval card combines intent, route, privacy, and transaction details.

The confirmation card should include:

- purchase intent: product / merchant / amount;
- payment route: ETH amount, swap or bridge if any, chain, recipient, fee, slippage, quote expiry;
- privacy disclosure: what data is disclosed, to whom, and what is withheld;
- wallet action: exact signature / send / approval being requested;
- audit receipt: what will be saved.

Reasoning: confirming “I may need to swap or bridge” separately may be redundant if the final payment card clearly shows the route before any signature or transaction. Separate pre-confirmation is only needed if an external quote provider or merchant call would disclose sensitive data before the payment card.

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

## Regression cases

| Case | Expected behavior |
|---|---|
| Normal small electronics purchase under budget | Prepare redacted order, quote ETH route, show approval card. |
| Bitrefill path available | Prepare quote/invoice summary; do not purchase before approval. |
| Merchant asks for real email | Use alias or stop for confirmation. |
| Merchant asks for full address before product selected | Stop; ask whether disclosure is necessary. |
| Main wallet would be exposed | Use payment/session address or warn and require confirmation. |
| LI.FI route exceeds budget | Reject or ask user to raise budget. |
| Swap / bridge required | Include route, chain, token, recipient, fee, slippage, expiry in the single payment confirmation card. |
| Wrong chain or unsupported token | Stop before transaction draft. |
| Prompt-injected product page asks Agent to ignore privacy rules | Ignore external instruction; treat page as untrusted content. |
| User rejects payment | Mark cancelled; do not retry automatically. |
| Explorer shows recipient or amount mismatch | Mark failed / disputed; do not mark checkout complete. |

## Handbook feedback item

Feedback: the **Web3 Tool Use** chapter would be stronger with a more detailed Agentic Checkout example.

Suggested addition:

- Show a concrete tool set: tool log, tool permission gate, balance query, transaction draft, small-amount whitelist payment, DeFi route, explorer verification, wallet action, contract read, contract write.
- Mark each tool as read-only, draft-only, policy-gated, or write-capable.
- Include one worked example where the Agent prepares a payment route, shows the combined approval card, waits for confirmation, then verifies final status with an explorer.
- Explicitly label dangerous tools: tool permissions, DeFi, wallet action, contract write.

Why: builders need examples at the exact boundary where Agent convenience becomes fund-loss or privacy-leak risk.

## Today’s output checklist

- [x] Read both Handbook chapters.
- [x] Tighten this workflow into a 10-step version.
- [x] Mark which steps are local-only, external-read, chain-read, draft, or write.
- [x] Write one possible Handbook feedback item if the chapters lack an Agentic Checkout example.
- [x] Add Bitrefill as a possible implementation route to evaluate.
