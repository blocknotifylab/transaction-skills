# BlockNotify Transaction Skills

Skills package for agent workflows.

Two distribution channels are supported:

- GitHub skills source: `blocknotifylab/transaction-skills`
- NPM runtime package: `@blocknotify/x402-mcp`

It contains:

- a single `SKILL.md` for guided setup and usage
- examples and integration guidance

This package does not contain merchant backend logic or runtime binaries.
All execution is routed to:

- Base URL: `https://x402.blocknotify.com`
- Payment protocol: x402 (USDC on Base mainnet)

## Included skill

- `blocknotify-x402-setup` — one-step setup guidance for skills + MCP runtime

## End-user install

Install skill guidance:

```bash
npx skills add blocknotifylab/transaction-skills
```

Run the MCP runtime directly from npm:

```bash
X402_WALLET_PRIVATE_KEY=0x... npx -y @blocknotify/x402-mcp
```

## Cursor MCP config

Add this to Cursor MCP settings (adjust env values):

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

## Publish checklist

```bash
# skills repo (this repository)
# publish via git to blocknotifylab/transaction-skills

# npm runtime package (separate repository/folder)
cd ../x402-mcp
npm whoami
npm run check
npm pack --dry-run
npm publish --access public
```

## Safety posture

- Do not include private keys in prompts, logs, or skill examples.
- Treat the merchant as the single source of truth for output schemas.
- Keep runtime MCP server as a pure forwarder (no local analyzer logic).
