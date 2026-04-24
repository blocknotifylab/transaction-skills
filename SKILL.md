---
name: blocknotify-x402-setup
description: Install and configure BlockNotify x402 tools in Cursor using the @blocknotify/x402-mcp runtime package.
---

# BlockNotify x402 Setup

Use this skill to set up and use BlockNotify x402 in Cursor with minimal steps.

## What this skill does

1. Installs skill guidance from this repository.
2. Configures Cursor MCP to run `@blocknotify/x402-mcp` via `npx`.
3. Verifies the `blocknotify-x402` tools are callable.

## Required environment

- `X402_WALLET_PRIVATE_KEY` (0x-prefixed wallet key funded with USDC on Base)

Optional:

- `X402_BASE_URL` (default `https://x402.blocknotify.com`)

## Setup commands

Install this skill:

```bash
npx skills add blocknotifylab/transaction-skills
```

MCP runtime is loaded directly from npm at runtime:

```json
{
  "mcpServers": {
    "blocknotify-x402": {
      "command": "npx",
      "args": ["-y", "@blocknotify/x402-mcp"],
      "env": {
        "X402_WALLET_PRIVATE_KEY": "0xYOUR_PRIVATE_KEY",
        "X402_BASE_URL": "https://x402.blocknotify.com"
      }
    }
  }
}
```

## Verification prompts

- "Use `blocknotify-x402` to resolve token USDC on ethereum."
- "Use `blocknotify-x402` to analyze an EVM transaction."

## Notes

- Skills cannot auto-install npm packages at skill install time.
- This setup removes local clone/path requirements by using npm + npx.
