# MetamorphosisEngine

When a prediction market resolves, it doesn't just end — it **metamorphoses**.

The expired market transforms into a permanent tradeable token on Bags.fm (Solana), carrying its AI-verified verdict, agent reasoning, dissent record, and origin story as immutable metadata. The market didn't die. It evolved into something new.

---

## The Concept

Most prediction markets leave resolved markets as dead data. A market about "Will BTC hit $100K?" that resolved YES becomes a historical record nobody can interact with.

BlockForecast turns that resolved market into a token — `$BTC100K` — that lives forever on Solana. Everyone who traded in the market gets a piece. The creator earns 1% on every future trade. The verdict is embedded in the token's DNA.

This is a new asset class: **oracle-verified tokens**. Every token has a provable origin story, AI-verified factual backing, and a built-in community of people who already believed in the outcome.

---

## Metamorphosis Criteria

Not every market metamorphoses. The following thresholds must be met:

| Criterion | Threshold | Why |
|-----------|-----------|-----|
| Oracle verdict | YES or NO (not void) | Can't birth a token from an unresolved question |
| Volume | ≥ $100 total traded | Minimum economic activity — proves the market mattered |
| Unique traders | ≥ 10 | Built-in community must exist before token launches |

If any criterion is unmet, the market resolves normally (payouts happen) but no token is launched.

---

## Token Metadata

Every metamorphosed token carries the following in its on-chain description:

```
Oracle Verdict: YES
5/6 agents · 94% confidence · 12.3 seconds
Resolved by 7 independent AI agents from 7 providers.
187 traders · $45,230 volume
Dissent: The Devil's Advocate voted NO (65% confidence).
Origin: BlockForecast BF-0042 on Base
```

This metadata is permanent. The token's history cannot be revised.

**Ticker generation:** The system extracts the key asset and price target from the market question and builds a ticker. "Will Bitcoin hit $100K before May 2026?" → `$BTC100K`. "Will ETH reach $10K?" → `$ETH10K`. This makes tokens immediately recognizable.

---

## Token Distribution

| Allocation | % | Purpose |
|-----------|---|---------|
| Liquidity Pool | 65% | Permanent tradeable liquidity on Bags |
| Community Airdrop | 25% | Distributed to market participants |
| Market Creator | 5% | Reward for creating the original market |
| Treasury | 5% | Platform sustainability |
| **Perpetual fee** | **+1%** | Bags' native creator royalty on all future trading |

The 65% LP allocation is burned into the pool — no one can remove it. The market has permanent liquidity from day one.

The community airdrop goes to everyone who traded in the original market, proportional to volume. They bought YES or NO shares and were proven right (or wrong) — either way, they get a share of the token that came from their participation.

The creator earns 5% of the initial supply **plus** 1% on every trade forever. The original market might have been $50K in volume. If the `$BTC100K` token goes viral and trades $50M on Bags, the creator earns $500K in perpetual fees from a market they created with a $1 fee.

---

## The Bridge: Oracle Launcher

The technical bridge between BlockForecast (Base L2) and Bags.fm (Solana) is **Oracle Launcher** — an autonomous bridge service.

```
BlockForecast Engine (Base)          Oracle Launcher          Bags.fm (Solana)
┌─────────────────────────┐    ┌──────────────────────┐    ┌──────────────────┐
│  Market resolves        │    │  Polls for resolved  │    │  Token deploys   │
│  Oracle: 5/6 YES        │───▶│  markets meeting     │───▶│  automatically   │
│  $45K volume, 187       │    │  metamorphosis       │    │                  │
│  traders                │    │  criteria            │    │  1% fee sharing  │
│                         │    │                      │    │  Community gets  │
│  Threshold met ✓        │    │  Builds metadata     │    │  tokens via      │
│                         │    │  Calls Bags SDK      │    │  airdrop         │
└─────────────────────────┘    └──────────────────────┘    └──────────────────┘
```

Oracle Launcher is fully autonomous. Once a market meets the criteria, the token launches without any human pressing a button.

See: [oracle-launcher-bags](https://github.com/gr8estman/oracle-launcher-bags)

---

## Why This Matters

**For traders:** Your prediction position, win or lose, becomes a stake in the token the prediction created. A market about BTC you traded in becomes a `$BTC100K` token you hold. Your DeFi activity compounds.

**For creators:** Creating a market isn't just a one-time event. If your market metamorphoses, you earn perpetually from every trade of the token forever. A $1 creation fee can yield a lifetime income stream.

**For Bags / Solana:** BlockForecast resolves 5–10 markets per day. That's a continuous pipeline of AI-verified, community-backed tokens flowing into Bags. Each one arrives with a built-in holder base, a verifiable origin story, and real trading history. Not anonymous launches. Not rugs. Tokens with provenance.

**For the space:** Oracle-verified tokens are a new category. Every token currently trading on Solana is just a claim. `$BTC100K` is different — it's a token that *is* the permanent record of an AI-verified fact.
