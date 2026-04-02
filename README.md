# BlockForecast V4

> Autonomous prediction markets on Base L2.
> Markets resolve themselves. Tokens are born from consensus.
> No human touches the outcome.

Three versions of a Solana trading terminal came first. This is what they were building toward.

Built entirely solo. Zero external funding. Testnet deployed and processing real trades.

---

## What It Is

A prediction market platform where:

1. Anyone creates a binary market ("Will X happen before Y date?")
2. Traders buy YES/NO shares using USDC
3. When the market expires, 7 independent AI agents from 7 different providers analyze it, deliberate independently, and reach consensus — autonomously, in ~12 seconds
4. The market resolves. Winners are paid out.
5. If the market had enough volume and traders, it **metamorphoses** — the expired market becomes a permanent tradeable token on Bags.fm (Solana), carrying its AI-verified verdict and origin story forever

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         BlockForecast V4                            │
├───────────────┬─────────────────────┬───────────────────────────────┤
│  Web Frontend │    Fastify Engine   │       Base L2 (Solidity)      │
│  Next.js 16   │  TypeScript / PG    │       Vault.sol (UUPS)        │
│  Tailwind v4  │  LSMR + CLOB hybrid │       USDC collateral         │
│  Privy auth   │  REST API + WS      │       On-chain settlement     │
├───────────────┴─────────────────────┴───────────────────────────────┤
│                         7-Agent AI Oracle                           │
│  Anthropic · OpenAI · Perplexity · DeepSeek · Groq · Ensemble      │
│  Independent deliberation · ~12s resolution · Self-improving        │
├─────────────────────────────────────────────────────────────────────┤
│                      MetamorphosisEngine                            │
│  Resolved market → Bags.fm token (Solana) · Perpetual fee sharing  │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Core Components

| Component | What It Does | Doc |
|-----------|-------------|-----|
| Trading Engine | Hybrid LSMR + CLOB, off-chain execution | [→ TRADING_ENGINE.md](docs/TRADING_ENGINE.md) |
| AI Oracle | 7 independent agents, multi-provider consensus | [→ ORACLE.md](docs/ORACLE.md) |
| Smart Contracts | Vault.sol on Base L2, UUPS proxy | [→ SMART_CONTRACTS.md](docs/SMART_CONTRACTS.md) |
| MetamorphosisEngine | Markets → Tokens on Solana | [→ METAMORPHOSIS.md](docs/METAMORPHOSIS.md) |
| Social Trading | Leaderboard, copy trade, follow system | [→ SOCIAL_TRADING.md](docs/SOCIAL_TRADING.md) |
| Creator Economics | $1 flat fee, 0.5% perpetual trading fees | [→ CREATOR_ECONOMICS.md](docs/CREATOR_ECONOMICS.md) |
| Frontend | All pages, real-time data, wallet flows | [→ FRONTEND.md](docs/FRONTEND.md) |

---

## Stack

**Blockchain**
- Solidity (UUPS proxies, ERC-7201 storage layout)
- Base L2 — low fees, EVM-compatible, Coinbase ecosystem
- Solana — token launch via Bags.fm and Oracle Launcher

**Backend**
- TypeScript + Fastify — high-throughput REST API + WebSocket
- PostgreSQL — positions, trades, LSMR state, oracle records, balances
- LSMR market maker + CLOB order book running fully off-chain

**Frontend**
- Next.js 16 (App Router, Turbopack)
- Tailwind v4 with CSS `@theme` variables
- Privy — embedded wallets, Base + Solana cross-chain auth
- React Query — optimistic updates, real-time invalidation

**AI / Oracle**
- Anthropic (Claude) — The Analyst + The Devil's Advocate
- OpenAI (GPT-4o) — The Skeptic
- Perplexity — The Investigator (live web search)
- DeepSeek-R1 — The Timeline Analyst (chain-of-thought)
- Groq (Llama) — The Crowd Reader
- Pure math ensemble — weighted aggregation, no LLM

---

## How We Got Here

V1, V2, and V3 were all Solana trading terminals — real-time token intelligence, risk scoring, MEV-protected execution. Three iterations of the same problem: how do you cut through the noise on a chain that mints 20,000 tokens a day?

By V3 the infrastructure was battle-tested. Kafka protobuf stream for sub-second detection. Jito bundles for execution. A 12-point rug scoring engine. The data pipelines were solid.

V4 is the pivot. Take that infrastructure — the data ingestion, the execution layer, the AI integration — and apply it to prediction markets. Not trading terminals. Not token snipers. A platform where anyone can create a binary market, trade it, and have it resolve autonomously via AI consensus.

| Version | What It Was |
|---------|-------------|
| [V1](https://github.com/gr8estman/BlockForecast) | Solana trading terminal. 12-Point Omniverse Math scoring engine, Ghost Mode paper trading, DeepChain forensics, Gemini AI research agent. |
| [V2](https://github.com/gr8estman/blockforecast-v2) | Terminal rebuilt with real-time dashboard — live market ticker, 2-second polling, BirdEye → BitQuery fallback, WebSocket broadcaster. |
| [V3](https://github.com/gr8estman/blockforecast-v3) | Full Next.js rewrite. Kafka protobuf sniper, Jito MEV bundles, pump.fun API, rug analysis suite, smart money tracker, Twitter bot. |
| **V4** | **The pivot. Autonomous prediction markets on Base L2. Everything the terminals taught, applied to a harder problem.** |

---

## Status

- Testnet deployed and processing real trades
- Oracle running: 7 agents, real API calls, real consensus
- Admin + Telegram bot operational
- MetamorphosisEngine implemented (Oracle Launcher — see [oracle-launcher-bags](https://github.com/gr8estman/oracle-launcher-bags))
- Frontend: all core pages live

---

[blockforecast.io](https://blockforecast.io) · [@blockforecasthq](https://x.com/blockforecasthq)
