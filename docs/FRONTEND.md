# Frontend

The BlockForecast V4 web app is a full trading platform. Every page has a specific job. The design system is consistent throughout — dark mode only, monospace numbers, green/red for probability, a visual language borrowed from trading terminals.

**Stack:** Next.js 16 (App Router, Turbopack), Tailwind v4 (`@theme` CSS variables), Privy (auth + embedded wallets), React Query (server state), Zustand (client state).

---

## Pages

### Home (`/`)

The first thing a new user sees. Not a landing page — a live market feed.

- Active markets sorted by volume, recency, and trending score
- Each market card shows: question, category, current YES probability, volume, trader count, time remaining
- Real-time probability updates via WebSocket — prices move as trades happen
- Category filters (Crypto, Politics, Sports, Tech, World Events)
- Search across all markets
- "Create Market" CTA for logged-in users

### Markets (`/markets`)

Full market browser. All markets, any state (open, resolved, metamorphosed).

- Advanced filtering: category, status, date range, volume
- Sort by: volume, newest, closing soon, most traders
- Pagination

### Market Detail (`/market/:id`)

The trading interface. This is where most user time is spent.

**Left panel:**
- Market question and description
- Resolution criteria (what exactly needs to happen for YES)
- Resolution date and source
- Current YES/NO probability (large, real-time)
- Price chart — probability history over time
- Recent trades feed

**Right panel (Order Form):**
- Buy YES / Buy NO toggle
- USDC amount input
- Real-time slippage preview — shows exact shares you'll receive at current price before you confirm
- Estimated return if correct
- Execute button

**Below the fold:**
- Your position (if you hold shares)
- All trades on this market (public, with trader addresses)
- Market comments
- Creator info and fee stats

**If resolved:**
- Final outcome (YES / NO)
- Oracle verdict with full agent breakdown — every agent's vote, confidence, and reasoning
- Dissent record
- Your payout (if any)
- Metamorphosis status (did it launch a token?)

### Create Market (`/create`)

4-step wizard:

1. **Question & Category** — Write the market question, select category, set event date. AI checks for similar existing markets to prevent duplicates. Auto-generates a ticker symbol.
2. **Resolution Details** — Write the resolution criteria (exactly what must happen for YES), set the source, confirm the resolution date.
3. **Creation Fee** — Static display: $1.00 fee, 0.5% trading fee you'll earn. No inputs needed.
4. **Review & Deploy** — Summary of all inputs. Checkbox to agree to terms. Deploy button charges $1 from balance and creates the market.

### Portfolio (`/portfolio`)

Your open positions across all markets.

- Each position: market question, side (YES/NO), shares held, current value, unrealized P&L
- Resolved positions: final outcome, what you won or lost
- Summary: total unrealized P&L, total realized P&L

### Creator Dashboard (`/creator`)

For users who have created markets. Shows earnings and market performance. See [CREATOR_ECONOMICS.md](CREATOR_ECONOMICS.md).

### Social Trading (`/social`)

Leaderboard and trade feed. See [SOCIAL_TRADING.md](SOCIAL_TRADING.md).

### Profile (`/profile/:address`)

Public trader profile. Shows stats, trade history, social links. Editable by the owner.

### Leaderboard (`/leaderboard`)

Top traders by various metrics. Links through to individual profiles.

### Activity (`/activity`)

Global activity feed — every trade, market creation, and resolution across the platform.

### Notifications (`/notifications`)

Per-user notification center:
- Market you're in is about to resolve
- Oracle has resolved a market you traded
- Trader you follow made a significant trade
- Your market has been created successfully
- Withdrawal processed

### Settings (`/settings`)

Account preferences:
- Notification toggles (email, browser push)
- Privacy settings (show/hide positions)
- Two-factor authentication
- Connected wallets

---

## Real-Time Data

Multiple layers of live updates:

**WebSocket subscriptions:**
- Market price feed — YES probability updates as trades execute
- Trade feed — new trades appear in real-time on market detail pages
- LP drain alerts — warning if the market pool is being drained unusually fast

**React Query polling:**
- Balance updates every 30 seconds
- Leaderboard refreshes every 60 seconds
- Oracle status during resolution

**Optimistic updates:**
- Trade submission: position updates immediately, reverts if the engine rejects
- Follow/unfollow: follower count updates instantly, reverts on failure
- Withdrawal: balance updates immediately

---

## Wallet & Auth

**Privy** handles all auth and wallet management.

- Email/social login → Privy creates an embedded EVM wallet (Base)
- Optional: connect existing Phantom or MetaMask
- Embedded wallets are cross-chain — the same Privy account links to a Base wallet and Solana wallet, enabling metamorphosis token airdrops to land in the right place

Users never need to manually sign USDC transfers for in-platform trading. The embedded wallet handles it silently. Only deposits from and withdrawals to external wallets require a manual signature.

---

## Design System

- **Dark mode only** — `#0a0a0b` background, `#1a1a1f` cards, `#2a2a35` borders
- **Accent:** Electric blue `#6366f1` for primary actions, green `#22c55e` for YES/profit, red `#ef4444` for NO/loss
- **Typography:** System sans-serif for UI, `font-mono` for all numbers
- **Animations:** `animate-in fade-in slide-in-from-bottom-4 duration-500` on page transitions, subtle pulse on live data
- **Components:** Custom card/button/input system built on Radix UI primitives
