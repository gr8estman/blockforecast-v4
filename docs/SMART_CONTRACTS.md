# Smart Contracts

BlockForecast V4 uses a minimal on-chain footprint. All trading logic runs off-chain. The smart contract's only job is to be an **honest custodian of user funds**.

Chain: **Base L2** (Coinbase's EVM L2 — low fees, high throughput, EVM-compatible)

---

## Design Philosophy

The biggest risk in prediction markets is a compromised resolution system stealing funds by manipulating outcomes. BlockForecast eliminates this by making the smart contract resolution-agnostic.

The Vault doesn't know or care who won the market. It just holds USDC and releases it when the engine tells it to — and the engine can only instruct payouts that the solvency invariant allows.

---

## Vault.sol

The single deployed contract. A USDC custodian with upgrade capability.

**What it does:**
- Accepts USDC deposits from users
- Holds funds in custody during market lifetime
- Releases USDC on withdrawal instructions from the authorized engine operator
- Emits on-chain events for every deposit and withdrawal (publicly verifiable)

**What it doesn't do:**
- Execute trades (all off-chain)
- Know about individual markets (just total balances)
- Have any oracle logic
- Require any interaction to resolve markets

---

## Proxy Architecture: UUPS

Vault.sol is deployed behind a **UUPS (Universal Upgradeable Proxy Standard)** proxy.

```
User → Proxy (stable address) → Implementation (upgradeable)
```

**Why UUPS over Transparent Proxy:**
- Lower gas cost (upgrade logic is in the implementation, not the proxy)
- Simpler proxy bytecode
- Upgrade authorization is enforced in the implementation's `_authorizeUpgrade()` function — only the owner can upgrade
- If the implementation is replaced with a broken one, the proxy itself is still callable (vs. Transparent where a broken implementation can brick the proxy)

**Storage layout:** ERC-7201 namespaced storage. All state is stored in a deterministic slot derived from `keccak256("blockforecast.vault.storage")`. This prevents storage collisions between proxy and implementation across upgrades.

---

## Security Properties

**Re-entrancy:** All state changes happen before external USDC transfers (checks-effects-interactions pattern). Additionally uses OpenZeppelin's `ReentrancyGuard`.

**Flash loan resistance:** The contract does not use spot balances to make decisions. All accounting is tracked in internal state, not `IERC20.balanceOf()`.

**Operator authorization:** Only the authorized engine operator address can instruct withdrawals. This address is set at deployment and can be rotated by the owner.

**Solvency guarantee:** The engine enforces: `sum(all pending payouts) ≤ vault USDC balance`. If this invariant would break, the engine refuses new trades before they're submitted on-chain.

**Upgrade timelock:** Contract upgrades have a timelock enforced in the implementation. No instant upgrades — gives users time to exit if they disagree with a proposed change.

---

## USDC on Base

Base L2 uses Coinbase's bridged USDC. Contract address:
`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`

All positions, fees, and payouts are denominated in USDC. No native token, no governance token, no speculative element in the core platform.

---

## Event Log

Every significant action emits an on-chain event, creating a public audit trail:

| Event | When Emitted |
|-------|-------------|
| `Deposited(user, amount)` | User deposits USDC into vault |
| `Withdrawn(user, amount)` | User withdraws USDC from vault |
| `OperatorChanged(old, new)` | Engine operator address rotated |
| `UpgradeScheduled(impl, eta)` | Upgrade timelock started |
| `Upgraded(impl)` | New implementation activated |

These events are permanently on-chain and independently verifiable. Anyone can reconstruct the full deposit/withdrawal history without trusting BlockForecast.

---

## Deployment

**Network:** Base Mainnet (chain ID 8453) / Base Sepolia Testnet (chain ID 84532)

**Deployment process:**
1. Deploy implementation contract
2. Deploy UUPS proxy pointing to implementation
3. Call `initialize()` on proxy (sets owner, authorized operator, USDC address)
4. Transfer ownership to multisig (for production)
5. Verify both contracts on Basescan

**Auditing:** The contract is designed for auditability — minimal surface area, standard OpenZeppelin primitives, no custom cryptography.
