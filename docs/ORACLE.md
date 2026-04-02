# AI Oracle

The oracle is the most novel part of BlockForecast. It replaces the human resolution process found in every other prediction market platform with a **deliberation system** — 7 independent AI agents from 7 different providers analyzing the same question, never seeing each other's answers, producing a weighted consensus in ~12 seconds.

---

## The Problem With Human Resolution

Every other platform has the same weakness: a human (or committee) decides the outcome. This means:
- Resolution takes days or weeks
- Disputes arise when the human and the market disagree
- The resolver can be bribed, pressured, or simply wrong
- The process doesn't scale

BlockForecast removes humans from the loop entirely.

---

## The 7 Agents

| Agent | Provider | Analytical Role | Key Strength |
|-------|----------|-----------------|-------------|
| The Analyst | Anthropic (Claude) | Data-driven quantitative analysis | Charts, metrics, institutional data |
| The Skeptic | OpenAI (GPT-4o) | Bear case and risk factors | Counter-arguments, tail risks |
| The Devil's Advocate | Anthropic (high temperature) | Forced minority view | Deliberately argues against consensus |
| The Investigator | Perplexity | Real-time fact verification | Live web search, primary sources |
| The Timeline Analyst | DeepSeek-R1 | Temporal chain-of-thought | Date logic, sequence verification |
| The Crowd Reader | Groq (Llama) | Open-source consensus + sentiment | Polymarket data, social signals |
| The Ensemble | Pure math | Weighted aggregation | No LLM — just the final calculation |

**Critical design decisions:**
- Agents run on **different AI providers** — no single company can control the outcome
- Agents are **isolated** — they never see each other's reasoning (prevents groupthink)
- The Devil's Advocate uses **high temperature** — forced to argue the minority view even when evidence is overwhelming. Its dissent is permanently recorded.
- The Investigator has **live internet access** — it checks real-time prices, news, and on-chain data
- The Ensemble is **not an AI** — it's a mathematical function applied to the other 6 votes

---

## Resolution Flow

```
Market expires
      │
      ▼
Oracle triggered
      │
      ├──────────────────────────────────────────────┐
      │                                              │
      ▼ (parallel, ~2s each)                        ▼
Agent 1: The Analyst                         Agent 4: The Investigator
  Reads market question                        Web search: finds primary source
  Analyzes available data                      Verifies factual outcome
  Returns: vote, confidence, reasoning         Returns: vote, confidence, source URL
      │                                              │
      ▼                                             ▼
Agent 2: The Skeptic                         Agent 5: The Timeline Analyst
  Constructs the bear case                     Chain-of-thought date reasoning
  Finds counter-evidence                       Verifies question deadline vs outcome date
  Returns: vote (often reluctant), reasoning   Returns: vote, step-by-step logic
      │                                              │
      ▼                                             ▼
Agent 3: The Devil's Advocate                Agent 6: The Crowd Reader
  MUST argue against majority (high temp)      Checks Polymarket, social sentiment
  Records formal dissent                       Open-source model independent view
  Returns: minority vote, dissent statement    Returns: vote, market data
      │
      └──────────────────────────┐
                                 ▼
                        Agent 7: The Ensemble
                          Receives all 6 votes
                          Applies historical accuracy weights
                          Calculates weighted confidence score
                          Returns: final verdict (YES/NO/void)
                                 │
                                 ▼
                        Market resolves (~12s total)
```

---

## The Ensemble: How Weighting Works

The Ensemble applies accuracy weights derived from each agent's historical performance.

When a new oracle instance starts, all agents get equal weight (1/6 each).

After each resolution, the system compares each agent's vote to the final outcome and updates their accuracy score:

```
accuracy = (correct_votes) / (total_votes)
weight = accuracy / sum(all_accuracies)
```

Agents that consistently vote correctly gain influence. Agents that miss get downweighted. The system converges toward relying most on whichever providers are most accurate for the types of questions being asked.

**Calibration** — confidence scores are also tracked against outcomes. An agent that says "95% YES" and is wrong gets a larger accuracy penalty than one that said "55% YES." This rewards honest uncertainty.

---

## Void Verdict

If agents cannot reach consensus (no majority, or all confidence scores below threshold), the oracle returns `void`. All traders receive their original USDC back. The market is considered unresolvable.

Void trigger conditions:
- No majority (e.g. 3 YES, 3 NO among the 6 voting agents)
- Ensemble confidence below 60%
- The Investigator finds conflicting primary sources with no resolution

---

## The Dissent Record

Every time The Devil's Advocate votes against the majority, its dissent is permanently stored:
- The agent's vote
- Its confidence level
- Its dissent statement (the specific concern it raised)
- The final outcome (so you can see whether the dissent was warranted)

This creates a permanent record of what the oracle was uncertain about. If the Devil's Advocate was right, future weighting reflects it.

---

## Resolution Time

Average: **12–15 seconds** for all 6 agent analyses + ensemble calculation.

Breakdown:
- The Crowd Reader (Groq): ~0.8s
- The Investigator (Perplexity): ~1.2s
- The Skeptic (OpenAI): ~1.8s
- The Analyst (Anthropic): ~2.1s
- The Devil's Advocate (Anthropic, high temp): ~3.2s
- The Timeline Analyst (DeepSeek-R1): ~4.5s

Agents run in parallel. Total wall-clock time = slowest agent + ensemble calculation overhead.

---

## Comparison to Other Resolution Systems

| System | Resolution Time | Human Required | Disputes Possible | Cost |
|--------|----------------|----------------|-------------------|------|
| BlockForecast Oracle | ~12 seconds | No | No | Gas only |
| Polymarket | Days–weeks | Yes (manual) | Yes | Gas + dispute fees |
| UMA Optimistic Oracle | 2+ hours | Yes (if challenged) | Yes | Dispute bond required |
| Chainlink | Minutes | No | No | Oracle node fees |

The key difference from Chainlink: Chainlink requires pre-existing data feeds. BlockForecast's oracle can resolve **any natural language question** — no data feed required. It reasons about the question the way a human expert would, but faster and without financial incentive to lie.

---

## Appeals

Traders who believe the oracle erred can submit an appeal. An appeal triggers a second oracle run with adversarial agents — agents specifically prompted to find flaws in the original reasoning.

Appeals are time-limited and require a stake. If the appeal succeeds (verdict flips), the appealing trader wins the stake. If it fails, the stake goes to treasury.
