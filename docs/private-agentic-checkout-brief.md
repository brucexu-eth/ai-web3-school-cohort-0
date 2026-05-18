# Private Agentic Checkout — Project Brief

> Working name: **Private Agentic Checkout**  
> Demo scenario: use ETH to buy an electronics item through a personal Agent, while minimizing personal-data leakage.  
> Track fit: Agentic Commerce / Payment + AI Security / Privacy.

## One-line thesis

Future users will ask personal Agents to buy goods for them. If checkout is delegated to an Agent without a privacy and permission layer, the user leaks purchase intent, contact data, wallet identity, asset state, and behavioral history to too many parties. **Private Agentic Checkout** proposes a structured checkout flow where intent, privacy policy, payment routing, user confirmation, and audit receipt are separated and explicit.

## Demo target

Use Hermes as the user-facing Agent:

```text
Hermes, buy me an electronics item under 0.02 ETH. Use ETH if possible. Do not expose my main wallet, real email, or unnecessary personal information.
```

Initial product category: **electronics**.

Good demo products:

- USB-C hub
- mechanical keyboard accessory
- charger / cable / adapter
- small hardware gadget

Reason: electronics are familiar, plausible for online purchase, relatively low value, and can be delivered physically or digitally tracked. Avoid medical, regulated, age-restricted, financial, or high-value goods.

## Why ETH

ETH is the native asset for the story: a user should be able to express a purchase budget in ETH, not only in fiat or stablecoins. The system may still route through LI.FI or another payment path if the merchant requires a different token or chain.

Important distinction:

- **User intent denomination**: ETH.
- **Payment route**: may be ETH direct, ETH → stablecoin, or cross-chain route through LI.FI.
- **Agent authority**: can prepare quote / route / transaction draft, but cannot spend without explicit user confirmation or a narrow policy.

## Product architecture

```text
User intent
→ Intent Parser
→ Privacy Policy Layer
→ Merchant / Product Adapter
→ Payment Route Planner (ETH / LI.FI)
→ Risk + Privacy Summary
→ Human Confirmation
→ Wallet / Smart Account Handoff
→ Audit Receipt
```

## Core modules

### 1. Intent Layer

Captures what the user actually authorizes.

Example fields:

- product category: electronics
- budget: max 0.02 ETH
- allowed payment asset: ETH preferred
- allowed route: direct ETH or LI.FI quote
- delivery constraints
- disclosure constraints
- confirmation threshold

### 2. Privacy Policy Layer

Classifies user data before any external call.

Disclosure classes:

- `public`: safe to reveal to all tools.
- `merchant_only`: reveal only if needed for checkout.
- `payment_provider_only`: reveal only to quote / route provider if needed.
- `agent_local_only`: stay inside Hermes / local store.
- `never_disclose`: never send externally.

Default rules:

- Do not expose the user's main wallet address to merchant if a payment/session address can be used.
- Do not expose real email; prefer alias.
- Do not expose full delivery address until a specific merchant checkout requires it.
- Do not send purchase history or private notes to LI.FI / route providers.
- Do not sign or broadcast any transaction before confirmation.

### 3. Payment Route Planner

For hackathon scope, this can begin as a mock or quote-only integration.

Inputs:

- source asset: ETH
- source chain
- budget
- merchant payment requirement
- max slippage / fee
- allowed route providers

Outputs:

- quote summary
- route steps
- expected received asset
- fees
- chain IDs
- recipient
- calldata / transaction draft if applicable

### 4. Confirmation Layer

The user should see one compact approval card:

- product / merchant
- max ETH spend
- token and chain changes
- recipient
- privacy fields disclosed
- information withheld
- risk flags
- cancellation option

### 5. Audit Receipt

A machine-readable record for later review:

- user intent hash / summary
- policy version
- data disclosed to each party
- payment quote / route
- user confirmation record
- transaction hash or mock receipt
- final status

## Minimum prototype boundary

In the first version, the project does **not** need to complete a real purchase.

Must demonstrate:

1. Parse a checkout intent.
2. Apply a privacy policy.
3. Produce a redacted order payload.
4. Produce an ETH-based payment route summary.
5. Generate a user confirmation card.
6. Generate an audit receipt.

Optional:

- LI.FI quote API integration.
- Mock merchant checkout.
- Transaction draft generation.
- Demo video.

Out of scope for v0:

- Real private shipping infrastructure.
- Full anonymous purchasing.
- Production wallet custody.
- Automatic transaction signing.
- General shopping search across the internet.

## Core insight to defend

A wallet confirmation is not enough for Agentic Commerce. The user also needs a **privacy confirmation** and an **intent confirmation**. In Agentic Checkout, approval should cover four things at once:

1. What is being bought.
2. What is being paid.
3. What personal data is being disclosed.
4. What verifiable receipt is being created.
