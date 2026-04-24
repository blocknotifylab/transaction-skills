---
name: x402-thin-mcp-client
description: Configure and use the thin BlockNotify MCP proxy package that forwards tool calls to x402 hosted REST APIs.
---

# x402 Thin MCP Client

Use this skill when an agent needs MCP tool ergonomics while keeping execution
as a thin proxy to hosted REST routes.

## Goal

Design a local MCP server that does only three things:

1. accepts MCP tool calls
2. forwards payloads to `https://x402.blocknotify.com`
3. returns the merchant JSON response

No analyzer logic should run locally.

## Included executable

Use the npm runtime package:

- `@blocknotify/x402-mcp`

Run:

```bash
X402_WALLET_PRIVATE_KEY=0x... npx -y @blocknotify/x402-mcp
```

## Tool mapping

Map each tool 1:1 to REST:

- `analyze_transaction` -> `POST /v1/analyze-transaction`
- `analyze_erc20_transfer` -> `POST /v1/analyze-erc20-transfer`
- `simulate_transaction` -> `POST /v1/simulate-transaction`
- `resolve_token` -> `POST /v1/resolve-token`

## Implementation constraints

- Keep transport `stdio` for MCP.
- Keep request/response schemas aligned with merchant contracts.
- Do not add hidden behavior that changes merchant semantics.
- Pass through errors cleanly with actionable text.

## Security constraints

- Never log raw private keys.
- Redact payment artifacts in debug logs unless explicitly needed.
- Keep env surface minimal:
  - required: `X402_WALLET_PRIVATE_KEY`
  - optional: `X402_BASE_URL` (default `https://x402.blocknotify.com`)

## UX requirements

- Tool descriptions must explain user outcome, not internal implementation.
- For paid calls, make it explicit that x402 settlement applies.
- If merchant returns a payment-required challenge, handle it and retry without
  exposing low-level complexity to end users.

## Release guidance

- Keep executable scope thin: MCP -> hosted REST forwarder only.
- Do not include internal service code, private test fixtures, or deployment
  configuration.
