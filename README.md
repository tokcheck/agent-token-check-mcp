# Agent Token Check — MCP server

<!-- mcp-name: io.github.mintnft04/agent-token-check -->

Remote MCP server that tells an AI agent whether a crypto token is safe to touch before it buys, swaps or approves it.

- **Endpoint:** `https://agent-token-check.pages.dev/mcp` (Streamable HTTP, stateless, no API key)
- **Server card:** `https://agent-token-check.pages.dev/.well-known/mcp.json`
- **Website / docs:** https://agent-token-check.pages.dev/

## Tools

| Tool | Input | What it returns |
|---|---|---|
| `check_token` | `token` (0x… EVM address, Solana mint, Tron T… address or Sui coin type), `chain` (optional, default `bsc`; Solana/Tron/Sui auto-detected) | Verdict **SAFE / CAUTION / DANGER**, a 0–100 score and plain-English reasons, each citing its source |
| `list_chains` | — | The 45 supported chains, their keys, and whether a live sell simulation is available |

What `check_token` looks at: honeypot (live sell simulation on Ethereum, BNB Chain, Base), buy/sell tax, owner powers (mint, pause, blacklist, change tax, hidden owner), Solana mint/freeze authority and Token-2022 extensions, proxy/upgradeable contracts, LP lock, top-holder concentration, source verification and age. Data: GoPlus Security, honeypot.is, RugCheck, public RPCs.

Chains include Ethereum, BNB Chain, Base, Arbitrum, Polygon, Optimism, Avalanche and 30+ other EVM chains, plus Solana, Tron and Sui.

## Pricing

- Free: 3 `check_token` calls per day per IP (`list_chains` is unlimited).
- After that, the tool returns the paid HTTP endpoint instead of an error: `GET https://agent-token-check.pages.dev/check?token=…&chain=…`, $0.01 USDC per call via x402 v2 (Base, BNB Chain, Polygon, Arbitrum, OP Mainnet, Avalanche). No account or key needed.

## Use with Claude / Cursor

Claude Desktop / Cursor / any client that supports remote MCP:

```json
{
  "mcpServers": {
    "agent-token-check": { "type": "http", "url": "https://agent-token-check.pages.dev/mcp" }
  }
}
```

Claude Code:

```
claude mcp add --transport http agent-token-check https://agent-token-check.pages.dev/mcp
```

Supported MCP protocol versions: 2025-11-25, 2025-06-18, 2025-03-26, 2024-11-05.

## Agent framework plugins

- ElizaOS plugin: https://github.com/mintnft04/plugin-agent-token-check
- GOAT SDK plugin: https://github.com/mintnft04/goat-plugin-agent-token-check

Automated heuristic from third-party data, not financial advice. A token that passes today can change tomorrow.
