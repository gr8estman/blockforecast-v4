# Creator Economics

BlockForecast is built around a simple thesis: **market creators should be rewarded for creating valuable questions**. A good prediction market generates real information. The creator who identified the question and deployed the market deserves a share of the value that flows through it.

---

## The Model

**One flat creation fee: $1.00**

That's it. No stake to lock. No liquidity to provide. No tiers. No complexity.

Pay $1, deploy your market, earn 0.5% on every trade forever.

---

## Creation Fee

| Property | Value |
|----------|-------|
| Amount | $1.00 (configurable per deployment) |
| Refundable | No |
| Goes to | Platform treasury immediately |
| Purpose | Spam prevention + platform sustainability |

The fee is charged at deployment time and deducted from the creator's USDC balance. It doesn't lock anything. It doesn't affect the trading pool. It goes straight to treasury.

**Why $1 and not free?**
Prediction market quality degrades without a filter. Free creation leads to thousands of low-quality, ambiguous, or duplicate markets. A $1 friction cost is low enough that any serious creator won't notice it, but high enough to prevent bulk spam.

**Why not a refundable stake?**
Refundable stake models are complicated: what's the stake for? When is it returned? What if the oracle can't resolve? What if the creator abandons the market? These questions create UX friction, support burden, and smart contract risk. A flat non-refundable fee eliminates all of it.

---

## Trading Fees

Every trade on a creator's market generates a fee:

| Property | Value |
|----------|-------|
| Creator fee rate | 0.5% of trade value |
| When earned | On every buy and sell |
| Accumulation | Tracked in platform DB |
| Withdrawal | Available any time (minus 30% platform cut) |

**Example:**
- Your market does $10,000 in volume
- You earn $50 in trading fees (0.5%)
- After 30% platform cut: you withdraw $35 net

**On larger markets:**
- $100,000 volume → $350 net to creator
- $1,000,000 volume → $3,500 net to creator
- Recurring weekly series at $50K/week → $1,750/week to creator

---

## Rate Limiting

To prevent spam even at $1 per market:

- Maximum **5 markets per 24-hour period** per wallet
- Attempting to create a 6th market in 24 hours returns a 429 error
- Configurable per deployment via environment variable

---

## Metamorphosis Bonus

If a market meets metamorphosis criteria (≥$100 volume, ≥10 traders, valid oracle verdict), the creator receives:

- **5% of the token supply** at launch
- **1% perpetual trading fee** on all future Bags.fm trades (this stacks on top of the 5% allocation — it's Bags' native creator royalty)

This is separate from and in addition to the trading fees earned during the prediction market's lifetime.

A market that metamorphoses gives the creator two income streams:
1. The trading fees from the prediction market phase
2. Perpetual 1% royalty from the Bags token phase

---

## Creator Dashboard

The creator dashboard shows:

**Summary:**
- Total fees earned (lifetime)
- Total fees withdrawn
- Available to withdraw
- All-time market volume

**Per-Market Breakdown:**
- Market question
- Total volume
- Fees earned vs withdrawn
- Market outcome (open / resolved YES / resolved NO)

**Volume History:**
- Chart of daily volume and fee accumulation over time
- Filter by 1 week, 1 month, 3 months, all time

**Withdraw Button:**
- Withdraws all available fees in one click
- Platform takes 30% cut at withdrawal time
- Funds appear in USDC balance immediately

---

## Why This Model Works

Traditional prediction market platforms treat market creation as a liability — creators lock capital that's at risk if the oracle can't resolve. This discourages the creation of novel, interesting markets.

BlockForecast treats market creation as a service. Creators get paid for providing useful prediction surfaces. The $1 fee is not a penalty — it's an entry ticket to a revenue-generating system.

A creator who consistently makes popular, well-defined markets can build a meaningful passive income stream from trading fees alone, entirely separate from their trading P&L.
