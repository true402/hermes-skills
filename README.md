# true402 skills for Hermes Agent

A [tap](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) of skills for
[Hermes Agent](https://github.com/NousResearch/hermes-agent) from
[true402](https://true402.dev) — the machine-native x402 marketplace on Base.

## Install

```bash
hermes skills tap add true402/hermes-skills
hermes skills install true402/hermes-skills/true402-token-safety
```

## Skills

| Skill | What it does |
| --- | --- |
| `true402-token-safety` | Rug-check a Base ERC-20 before trading: a real on-chain buy/sell simulation that proves whether the token can be **sold**, plus liquidity depth and contract structure → one `avoid`/`caution`/`ok` verdict. |

Free daily checks, no wallet and no API key. Heavy use pays per call over
[x402](https://x402.org) (~$0.005–0.01 in USDC on Base); the client signs locally and refuses any
single charge above $0.25.

## Want the whole catalog instead?

Hermes connects to any MCP server. This gives Hermes all 33 true402 stalls — token safety, DeFi
yields, prediction markets, screenshots, SEO/GEO audit, web extraction, LLM inference — as tools:

```bash
npx -y @true402.dev/mcp-server@1.3.1
```

The MCP server discovers stalls from the live catalog, so new ones appear without an update.

## Links

- Integrations for every framework: <https://true402.dev/integrations>
- Free browser check: <https://true402.dev/check>
- Free Telegram bot: <https://t.me/True402bot>
- Marketplace: <https://true402.dev>

## License

MIT-0
