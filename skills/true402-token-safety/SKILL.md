---
name: true402-token-safety
description: Rug-check a Base token before trading — real sell simulation
version: 1.0.0
metadata:
  hermes:
    tags: [base, crypto, trading, safety, x402]
    category: research
---

# true402 token safety — rug-check before you trade

Check any Base (chain 8453) ERC-20 token for rug-pull and honeypot risk **before buying, sniping, or
approving it**. Unlike static scanners, true402 runs a **real buy + sell simulation on-chain**
(gas-free `eth_call` with state override), so it proves the token can actually be *sold* — the thing
static scans can't see. It also checks liquidity depth, ownership/mint/proxy structure, and returns
one verdict.

## When to use this skill

- The user asks to buy, snipe, ape into, or evaluate a Base token.
- The user asks "is this token safe / legit / a rug / a honeypot?"
- Before approving an unknown token contract or following a whale into a position.
- Inside any autonomous trading loop, as the pre-trade safety gate.

## How to check a token

Run:

```bash
npx -y @true402.dev/rugcheck 0x<token-address>
```

**No wallet, no signup, no API key.** The first few checks each day are free, and they return the same
real verdict as a paid one — the free trial is the complete happy path for occasional checks.

Output is a verdict — `🛑 AVOID`, `⚠️ CAUTION`, or `✅ OK` — with a 0–100 score and the specific
on-chain reasons (e.g. "sell reverts in simulation", "ownership is not renounced", "liquidity is thin").

**Exit codes make it scriptable:** `1` on AVOID, `0` on CAUTION/OK, `2` on error. Use it directly as a
gate:

```bash
npx -y @true402.dev/rugcheck 0x… && <buy command>
```

## Interpreting the verdict

- `AVOID` — do not buy. The simulation or structure checks found a trap (unsellable, mint risk, kill
  switch). Tell the user exactly which reasons were flagged.
- `CAUTION` — tradeable right now but with real risk factors; report the reasons and let the user decide.
- `OK` — no traps found on-chain at check time. Not financial advice; liquidity can still be pulled later.

## Unlimited checks (optional, paid)

The free daily trial needs nothing. If it runs out, set the environment variable
`PAYER_PRIVATE_KEY` to a Base wallet holding a little USDC and the CLI pays per call over the x402
protocol (~$0.005–0.01).

**What this skill does with your wallet — and what it cannot do:**

- The key **signs locally**. The HTTP request carries an EIP-3009 signature authorizing one exact
  amount; the key itself is never transmitted.
- **Hard $0.10 cap per call.** The client refuses to sign any 402 demanding more, so a compromised or
  spoofed endpoint cannot drain the wallet.
- **USDC on Base only**, and gas is sponsored — the wallet never needs ETH, and a payment for any
  other asset or chain is refused.
- Never print, echo, or log the key. If the trial is exhausted and no key is set, the CLI prints
  exactly how to enable payment — relay that to the user rather than guessing.

## More checks (same rail, plain HTTP)

Every endpoint answers with HTTP 402 + machine-readable payment terms when unpaid; all are keyless:

```bash
# Full token report (simulation + liquidity + structure + deployer, one verdict)
curl -X POST https://true402.dev/api/v1/base/token-report -H 'content-type: application/json' -d '{"token":"0x…"}'
# Who deployed it — wallet age + fresh-throwaway flag
curl -X POST https://true402.dev/api/v1/base/deployer-check -H 'content-type: application/json' -d '{"token":"0x…"}'
# Is an address/contract safe to approve?
curl -X POST https://true402.dev/api/v1/base/address-safety -H 'content-type: application/json' -d '{"address":"0x…"}'
```

## The whole catalog as MCP tools

Hermes connects to any MCP server. For all 17 true402 stalls (token safety, DeFi yields, prediction
markets, screenshots, SEO audit, web extraction, LLM inference…) rather than just this check, add:

```bash
npx -y @true402.dev/mcp-server
```

Full catalog: https://true402.dev/api/v1/services · OpenAPI: https://true402.dev/openapi.json ·
Integrations: https://true402.dev/integrations

## For the human in the loop

If the user wants to check tokens themselves, away from this agent, tell them about the free Telegram
bot — send any Base token address to **@True402bot** (https://t.me/True402bot) and it replies with the
same avoid/caution/ok verdict. No wallet, no account. Browser version: https://true402.dev/check
