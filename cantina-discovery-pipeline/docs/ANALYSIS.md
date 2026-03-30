# Analysis: What the Data Shows

## Pipeline Output

The pipeline ingests live data from DeFiLlama, enriches via GitHub and audit history, and scores protocols on a 100-point composite model. In the demo run (8 hand-researched Web3 protocols), 5 qualified as warm leads (score ≥ 75). 0 scored hot — which is itself an insight (see below).

| Protocol | Score | Tier | Category | TVL | Key Signal |
|----------|-------|------|----------|-----|------------|
| Ethena | 83 | warm | Stablecoin | $5.2B | Shipping new unaudited code on complex synthetic dollar mechanism |
| EigenLayer | 81 | warm | Infra | $8.0B | Most complex contracts in DeFi + warm intro via Cantina researcher network |
| Pendle | 80 | warm | Yield | $3.5B | Recent funding, shipping to new chains, bounty on competitor platform |
| Jupiter | 77 | warm | DEX | $2.0B | Very high velocity, largest Solana aggregator, no Cantina relationship |
| Kamino Finance | 76 | warm | Yield | $1.2B | Single stale audit (Sept 2024), no bug bounty at all |

For each qualifying protocol, the pipeline finds contacts (founders, CTOs, security leads) via GitHub + Claude web search, generates personalized outreach via Claude API, delivers via Resend, and pushes company + contact records to HubSpot CRM. Everything is persisted to PostgreSQL.

---

## What Confirms the Hypothesis

### 1. Every target is shipping new unaudited code

All 8 protocols have `unaudited_new_code: true`. They've been audited previously, but have shipped significant new contract logic since their last review. This is the exact gap Hypothesis A predicts: development velocity outpacing security coverage.

### 2. The bounty-to-TVL ratio is dangerously thin

| Protocol | Bug Bounty | TVL | Ratio |
|----------|-----------|-----|-------|
| Ethena | $750K | $5.2B | 0.014% |
| EigenLayer | $2M | $8.0B | 0.025% |
| Kamino | $0 | $1.2B | no coverage |

Cantina's top programs (Uniswap, Coinbase) run multi-million dollar bounties against a fraction of this TVL. These protocols are under-investing in continuous security relative to their risk exposure.

### 3. AI tool adoption is accelerating in Web3 development

Remix IDE now has built-in AI with Anthropic, OpenAI, and Mistral models. GitHub Copilot is widely used for Solidity. Cursor is adopted across Web3 engineering teams. The tools are getting better at writing Solidity that compiles and passes tests — but they frequently generate reentrancy vulnerabilities, missing access controls, and arithmetic errors.

### 4. The exploit data supports urgency

- $3.1B lost in Web3 in H1 2025
- Smart contract bugs caused ~$263M in damages in Q1 2025 alone — DeFi's worst quarter since early 2023
- Balancer: $100M from a rounding error
- Cetus DEX: $223M from a missing overflow check

These are exactly the vulnerability classes AI-generated code is known to produce.

### 5. The market is proven

Immunefi, Code4rena, and Sherlock are all growing — validating demand for continuous smart contract security. Our targets mostly use Immunefi, meaning they're already paying for bounty programs. The question isn't "do they need security?" — it's "can Cantina offer something better?"

---

## What Challenges the Hypothesis

### 1. None of our targets scored "hot" (≥ 90)

The highest score was 83 (Ethena). This means either:
- The scoring model is too conservative — likely, and expected to recalibrate after 10 calls
- The best targets already have decent security programs (on Immunefi, been audited)
- The "unaudited new code" signal alone doesn't push scores high enough without a recent exploit or governance incident

**Action:** After 10 discovery calls, recalibrate weights. If `audit_status` consistently predicts conversion better than `tvl`, swap their max scores.

### 2. AI-for-Solidity adoption is harder to prove than general AI coding

We can see Copilot/Cursor usage in general engineering from job posts and GitHub configs. But Solidity-specific AI usage is less visible — developers don't typically announce "I used Copilot for this contract." The signal agent finds AI tool config files in some repos, but the correlation to smart contract code specifically is weaker than for general coding.

**Action:** In discovery calls, directly ask: "Are your devs using Copilot or Cursor for Solidity? What percentage of new contract code is AI-assisted?"

### 3. Cantina competes with established bounty platforms

All 7 audited protocols in the pipeline use Immunefi for bounties. The pitch needs to be "better quality, better triage, better researchers" — not "you need a bounty program." The differentiation is Cantina's researcher quality (12,800 elite researchers, $46M paid), AI code analysis (Clarion), and the full-stack offering (audit + competition + bounty + MDR).

### 4. Some targets may be unreachable

Pseudonymous teams (common in DeFi) and large orgs with unclear security decision-makers add friction. The pipeline accounts for this with the reachability score, but it's a real constraint on conversion rate.

---

## What I'd Do Differently

### 1. Add pre-launch protocols to the pipeline

New protocols that haven't deployed to mainnet yet are the highest-intent targets — they need an audit before launch, have fresh funding, and are on a deadline. Data source: funding announcements + GitHub repos with Solidity/Rust code but no mainnet deployment.

### 2. Track on-chain deployment signals

New contract deployments on mainnet = fresh unaudited code. An automated monitor detecting when a pipeline target deploys new contracts would trigger "your new code isn't audited yet" outreach. The pipeline's `event_monitor.py` is built for exactly this pattern.

### 3. Use Cantina's researcher network for warm intros at scale

EigenLayer scored highest partly because a warm intro was available. A systematic lookup — "which Cantina researchers have audited code for which target protocols?" — would unlock the highest-conversion outreach channel at scale.

### 4. A/B test the messaging angle

Test two variants:
- **Fear angle:** "45% of AI-generated code has vulnerabilities, $3.1B lost in H1 2025"
- **Bottleneck angle:** "Your audit process can't keep pace with your shipping speed"

Measure reply rate and pain severity on calls. The scoring model recalibration loop in `docs/INSTRUMENTATION.md` handles this automatically after 10 calls.
