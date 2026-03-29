# Analysis: What the Data Shows

## Pipeline Output Summary

Scored 8 real Web3 protocols. 5 qualified as warm leads (score ≥ 75). 0 scored as hot — which itself is an insight (see below).

| Protocol | Score | Tier | Category | TVL | Key Signal |
|----------|-------|------|----------|-----|------------|
| Ethena | 83 | warm | Stablecoin | $5.2B | Shipping new unaudited code on complex synthetic dollar mechanism |
| EigenLayer | 81 | warm | Infra | $8.0B | Most complex contracts in DeFi + warm intro available via Cantina researchers |
| Pendle | 80 | warm | Yield | $3.5B | Recent funding, shipping to new chains, bounty on competitor platform |
| Jupiter | 77 | warm | DEX | $2.0B | Very high velocity, largest Solana aggregator, no Cantina relationship |
| Kamino Finance | 76 | warm | Yield | $1.2B | Single stale audit (Sept 2024), NO bug bounty at all |

## What Confirms the Hypothesis

### 1. Every target is shipping new unaudited code

All 8 protocols in the pipeline have `unaudited_new_code: true`. They've been audited previously, but have shipped significant new contract logic since their last review. This is the exact gap Hypothesis A predicts: development velocity outpacing security coverage.

### 2. The bounty-to-TVL ratio is dangerously thin

- Ethena: $750K bounty protecting $5.2B = 0.014% of TVL
- EigenLayer: $2M bounty protecting $8B = 0.025% of TVL
- Kamino: $0 bounty protecting $1.2B = **no incentive for whitehats at all**

For comparison, Cantina's top programs (Uniswap, Coinbase) run multi-million dollar bounties. The protocols in our pipeline are under-investing in continuous security relative to their risk exposure.

### 3. AI tool adoption is accelerating in Web3 development

Remix IDE now has built-in AI (RemixAI) with Anthropic, OpenAI, and Mistral models. GitHub Copilot is widely used for Solidity development. Cursor is adopted in Web3 engineering teams. The tools are getting better at writing Solidity that compiles and passes tests — but research shows they frequently generate reentrancy vulnerabilities, missing access controls, and arithmetic errors.

### 4. The exploit data supports urgency

- $3.1B lost in Web3 in H1 2025
- Smart contract bugs caused ~$263M in damages in Q1 2025 alone
- Balancer: $100M from a rounding error
- Cetus DEX: $223M from a missing overflow check
- These are exactly the vulnerability classes AI-generated code is known to produce

### 5. The market is proven (competitors are growing)

Immunefi, Code4rena, and Sherlock are all growing — validating demand for continuous smart contract security. Our targets mostly use Immunefi, meaning they're already paying for bounty programs. The question isn't "do they need security?" — it's "can Cantina offer something better?"

## What Challenges the Hypothesis

### 1. None of our targets scored "hot" (90+)

The highest score was 83 (Ethena). This means either:
- Our scoring model is too conservative (likely — we should tune after calls)
- The best targets already have decent security programs (they're on Immunefi, they've been audited)
- The "unaudited new code" signal alone doesn't push scores high enough

**Action:** After 10 calls, recalibrate weights. If `audit_status` consistently predicts conversion better than `tvl`, swap their weights.

### 2. AI-for-Solidity adoption is harder to prove than general AI coding

We can see Copilot/Cursor usage in general engineering from job posts and GitHub configs. But specific Solidity AI usage is less visible — developers don't typically announce "I used Copilot for this contract." The signal agent found AI tool config files in some repos, but the correlation to smart contract development specifically is weaker than for general coding.

**Action:** In discovery calls, directly ask: "Are your devs using Copilot or Cursor for Solidity? What percentage of new contract code is AI-assisted?"

### 3. Cantina competes with established bounty platforms

All 7 audited protocols in our pipeline use Immunefi for bounties. Cantina's pitch needs to be "better quality, better triage, better researchers" — not "you need a bounty program." The differentiation is Cantina's researcher quality (12,800 elite researchers, $46M paid), AI code analysis (Clarion), and the full-stack offering (audit + competition + bounty + MDR).

### 4. Some targets may be unreachable

Hyperliquid's team is pseudonymous. LayerZero's CEO is reachable but the org is large. For DAO-governed protocols, the "decision maker" might be a governance vote, not a person. The pipeline accounts for this with the reachability score, but it adds friction to the outreach process.

## What I'd Do Differently

### 1. Add more "pre-launch" protocols to the pipeline

New protocols that haven't deployed to mainnet yet are the highest-intent targets — they NEED an audit before launch, have budget from recent funding, and are on a deadline. Data source: funding round announcements + GitHub repos with Solidity/Rust code but no mainnet deployment.

### 2. Track on-chain deployment signals from Etherscan/Solscan

New contract deployments on mainnet = fresh unaudited code. An automated monitor that detects when a pipeline target deploys new contracts would be a trigger for "your new code isn't audited yet" outreach.

### 3. Use Cantina's researcher network for warm intros at scale

EigenLayer scored highest partly because a warm intro was available. Building a systematic lookup: "which Cantina researchers have audited code for which target protocols?" would unlock the highest-conversion outreach channel.

### 4. A/B test the messaging angle

Test two variants:
- **Fear angle:** "45% of AI-generated code has vulnerabilities, $3.1B lost in H1 2025"
- **Bottleneck angle:** "Your audit process can't keep pace with your shipping speed"

Measure which gets more responses and higher pain severity on calls.
