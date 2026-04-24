# x402 REST Caller

Use this skill when an agent needs to evaluate an EVM transaction before
execution by calling the BlockNotify hosted x402 merchant.

## Purpose

Help the model choose the correct route, construct valid request bodies, and
interpret response fields from:

- `https://x402.blocknotify.com`

## Endpoints

### 1) `POST /v1/analyze-transaction`

Use when the caller already has raw tx fields (`to`, `data`, `value`) and needs
a decision (`allow | warn | block`) plus findings and simulation output.

Required request fields:

- `chain`
- `from`
- `to`

Optional:

- `data` (default `0x`)
- `value` (default `0`)

### 2) `POST /v1/analyze-erc20-transfer`

Use for human-friendly token transfer checks.

Required request fields:

- `from`
- `recipient`
- `amount`
- one of `tokenSymbol` or `tokenAddress`

Optional:

- `chain` (default `ethereum`)
- `decimals`

### 3) `POST /v1/simulate-transaction`

Use when the caller needs raw execution output only (no allow/warn/block
decision).

Input shape matches `analyze-transaction`.

### 4) `POST /v1/resolve-token`

Use to normalize token identity and decimals before composing tx calls.

Request requires one of:

- `symbol`
- `address`

Optional:

- `chain` (default `ethereum`)

## Response handling rules

- Treat `simulation.success` and `simulation.gasUsed` as canonical.
- Treat `simulation.raw` as extensible; ignore unknown keys safely.
- Surface `findings[]` and `action` directly to downstream UX logic.

## Payment notes

- Paid routes are x402-protected.
- Clients should be x402-aware and handle the payment challenge/response flow.
- Settlement asset is USDC on Base mainnet.

## Output style for calling agents

When returning guidance, provide:

1. selected endpoint
2. request payload skeleton
3. key response fields to check
4. one-line integration caution (x402 settlement required)
