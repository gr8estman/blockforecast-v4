# Social Trading

BlockForecast V4 includes a full social layer built on top of the prediction market activity. Every trade is public. Every position is tracked. The best predictors rise to the top. Anyone can follow them, copy their trades, and learn from their reasoning.

---

## Leaderboard

A ranked table of all traders sorted by prediction performance.

**Metrics tracked per trader:**

| Metric | How It's Calculated |
|--------|-------------------|
| All-Time Return | `(total P&L) / (total capital deployed) × 100` |
| 30-Day Return | Same formula, rolling 30-day window |
| Win Rate | `(markets predicted correctly) / (total resolved markets traded)` |
| Sharpe Ratio | `mean(daily returns) / stddev(daily returns)` — measures return quality |
| Followers | How many traders are following this account |
| Risk Level | Conservative / Moderate / Aggressive based on avg trade size |

**Sort options:** All-Time Return, 30-Day Return, Win Rate, Followers, Sharpe Ratio

**Risk classification:**
- Aggressive: avg trade size > $100
- Moderate: avg trade size > $30
- Conservative: all others

---

## Trade Feed

A real-time stream of trades across the platform, with the option to filter to "followed traders only."

**Each feed item shows:**
- Trader username + avatar
- Market question
- Side (YES / NO)
- Amount in USDC
- Current probability at time of trade
- Whether the market is still open or resolved

The feed is the fastest way to see what smart money is doing. If 5 of the top 10 leaderboard traders just bought YES on a market, that's a signal.

---

## Follow System

Any trader can follow any other trader. Following gives you:
- Their trades appear in your "Following" feed tab
- You get notified when they make a significant trade
- Their profile shows their full stats, history, and positions

Follower/following counts are public and factor into leaderboard visibility.

---

## Copy Trading

Clicking "Copy" on any trade in the feed opens the Copy Trade modal:

**Settings:**
- Amount to deploy (in USDC)
- Auto-copy toggle: automatically mirror this trader's future trades on this market
- Copy ratio: if they trade $100, you trade X% of that

Copy trades execute at the current LSMR price at time of execution — not the copier's price. There's no guaranteed same entry.

---

## Trader Profiles

Every trader has a public profile page showing:

**Stats:**
- Win rate, total P&L, 30-day return, markets traded
- Follower count, following count
- Risk level (Conservative / Moderate / Aggressive)

**Trade History:**
- Every resolved market they traded
- Their position (YES/NO), amount, outcome
- Profit or loss per trade

**Social links:** Twitter, GitHub, website, location — whatever they choose to display

**Edit Profile:** Users can set a username, bio, avatar, and social links. These are stored in the platform database and shown to other traders.

---

## Why Social Trading Matters for Prediction Markets

Prediction markets are fundamentally about aggregating information. The price of YES on a market is the crowd's best estimate of the probability. But the crowd isn't uniform — some traders are consistently more accurate than others.

Social trading surfaces those traders. A new user who doesn't know whether to bet YES or NO on a geopolitical market can look at the leaderboard and see that the top 3 performers by Sharpe ratio all bought YES — and they can copy that trade immediately.

This creates a feedback loop: accurate traders build followings, their trades move markets toward correct prices faster, and the oracle's job becomes easier (the Crowd Reader agent already incorporates Polymarket data — eventually BlockForecast's own social signal will be an input).
