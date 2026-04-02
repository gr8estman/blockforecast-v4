# Trading Engine

BlockForecast uses a **hybrid LSMR + CLOB** trading model — the same architecture pioneered by Polymarket. All trade execution happens off-chain in the engine's database. The smart contract (Vault.sol) holds USDC collateral but never executes individual trades.

---

## Why Off-Chain Execution

On-chain AMM markets have two problems: gas costs make small trades uneconomical, and front-running is trivial. Running execution off-chain with on-chain settlement gives traders the UX of a CEX with the trust guarantees of a DEX. Balances are always redeemable on-chain. The engine cannot steal funds — only the user can withdraw.

---

## LSMR (Logarithmic Market Scoring Rule)

LSMR is the primary liquidity mechanism. The market always has a price. You can always trade. No counterparty needed.

**How it works:**

The market maintains two quantities: `q_yes` (total YES shares purchased) and `q_no` (total NO shares purchased). The cost function determines how much USDC a trade costs:

```
C(q_yes, q_no) = b × ln(e^(q_yes/b) + e^(q_no/b))
```

Where `b` is the liquidity parameter — higher `b` means less price impact per trade.

**Current price of YES:**
```
P(yes) = e^(q_yes/b) / (e^(q_yes/b) + e^(q_no/b))
```

The cost to buy `Δ` YES shares is `C(q_yes + Δ, q_no) − C(q_yes, q_no)`.

**Properties:**
- Maximum loss to the market maker is bounded at `b × ln(2)` (the liquidity parameter)
- Price always stays between 0 and 1 (representing probability)
- The more shares bought in one direction, the higher the price moves — just like an AMM

---

## CLOB (Central Limit Order Book)

The CLOB runs alongside LSMR as a secondary layer. Sophisticated traders can place limit orders — "buy YES at 0.65 or better."

**How it interacts with LSMR:**
- If a limit order crosses with LSMR's current price, it executes against the LSMR pool
- If it doesn't cross, it rests on the book waiting for another trader to fill it
- Limit orders get price improvement when LSMR moves toward them

**Order types supported:**
- Market order (executes immediately against LSMR)
- Limit order (rests on book or fills immediately if crossing)
- Cancel

---

## Trade Lifecycle

```
Trader submits trade
        │
        ▼
Engine validates:
  - Sufficient USDC balance
  - Market is open
  - Within slippage tolerance
        │
        ▼
Price calculated from current q_yes / q_no
        │
        ▼
USDC deducted from trader's off-chain balance
Shares credited to trader's position
q_yes / q_no updated in DB
        │
        ▼
Fee split:
  - 0.5% to market creator (always)
  - Remainder stays in pool for resolution payout
        │
        ▼
Position recorded for resolution payout
```

---

## Resolution & Payout

When the oracle resolves a market to YES or NO:

1. All positions on the winning side receive `$1.00 per share`
2. All positions on the losing side receive `$0.00`
3. The engine calculates each trader's payout from their position records
4. Funds are available for withdrawal immediately
5. The creator's accumulated trading fees are added to their withdrawal balance

If the oracle returns `void` (no consensus), all traders receive their original USDC back.

---

## Solvency

The engine maintains a solvency invariant: total USDC in the vault must always cover the maximum possible payout. This is enforced at the DB level — the engine checks solvency before processing each trade and refuses the trade if it would make the pool insolvent.

A solvency monitor runs continuously and alerts on any deviation.

---

## Series Markets

A series is a collection of recurring markets on the same question at different intervals (weekly, monthly). Example: "Will BTC be above $100K this week?" repeated every 7 days.

- Series markets share a `series_id`
- Each individual market in the series resolves independently
- The oracle treats each as a standalone resolution task
- The $1 creation fee is charged once per series (not per market)

---

## Liquidity Parameter (`b`)

`b` controls how much the price moves per trade. Higher `b` = more stable prices but higher maximum loss for the platform.

Current default: `b = 100` USDC equivalent units.

This means:
- A $10 trade moves the price by a small amount (roughly 0.1% on a 50/50 market)
- A $1,000 trade has significant price impact
- Maximum platform loss per market is `b × ln(2) ≈ 69 USDC`
