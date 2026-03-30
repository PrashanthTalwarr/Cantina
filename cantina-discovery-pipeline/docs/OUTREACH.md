# Outreach: Personalized Messages to Real Targets

## How It Works

The outreach agent (`src/agents/outreach_agent.py`) uses Claude API to generate a personalized cold email for every contact the pipeline finds. Each message is unique to the person and protocol — not a template with names swapped in.

**Signal → Draft → Send → CRM flow:**
1. Pipeline enriches each protocol (TVL, audit history, GitHub velocity, funding, team contacts)
2. Claude generates a personalized email using that enrichment data
3. Email is delivered via Resend
4. Contact + company pushed to HubSpot with outreach status set to `ATTEMPTED_TO_CONTACT`
5. Full record persisted to PostgreSQL

---

## Outreach Philosophy

Every message follows: **Signal** (something specific I noticed) → **Data** (third-party evidence of why it matters) → **Bridge** (how Cantina maps to this) → **Soft ask** (15 min, specific value offered).

Category-specific risk hooks make each message feel researched, not templated:

| Category | Risk Hook |
|----------|-----------|
| DEX | MEV extraction, reentrancy in swap logic |
| Lending | Oracle manipulation, liquidation edge cases |
| Yield/Vault | Accounting errors, withdrawal race conditions |
| Bridge | Cross-chain message validation, replay attacks |
| Stablecoin | Depeg scenarios, minting function access control |
| L2/Infra | Sequencer downtime, fraud proof edge cases |

---

## Example 1: Ethena (Stablecoin, $5.2B TVL)

**To:** Guy Young, Founder
**Channel:** Twitter DM
**Signals used:** $5.2B TVL · Audited by Quantstamp/Pashov but shipping new unaudited code · Very high velocity · Immunefi bounty at $750K

> Hey Guy —
>
> Ethena is holding $5.2B in TVL across a synthetic dollar mechanism that's inherently complex — delta-neutral positions, cross-exchange hedging, minting/redemption logic. That's a lot of surface area.
>
> With the shipping velocity I'm seeing on GitHub, you're deploying new contract logic faster than your last Quantstamp audit covers. The Immunefi bounty is a good start, but at $750K for $5.2B in TVL, the ratio is thin — especially when AI tools are accelerating how fast Solidity gets written across the industry.
>
> Cantina's platform runs the largest security competitions in DeFi ($46M+ paid out) and our AI analyzer catches patterns in minting/access control logic that standard tools miss. Same researcher network that secures Coinbase and Uniswap.
>
> Worth 15 min to compare notes on your security roadmap for 2026?

**Why this works:** References their specific mechanism (delta-neutral, minting), their exact bounty amount, their audit providers by name, and the TVL-to-bounty ratio. No generic "you should care about security" language.

---

## Example 2: Kamino Finance (Yield/Solana, $1.2B TVL)

**To:** Gabe, Co-Founder
**Channel:** Twitter DM
**Signals used:** $1.2B TVL · Single audit by OtterSec (Sept 2024) · No bug bounty · Concentrated liquidity vaults on Solana

> Hey Gabe —
>
> Kamino is sitting at $1.2B in concentrated liquidity vaults on Solana with one OtterSec audit from September 2024 and no active bounty program. That's 18 months of new code with no external review.
>
> For context: $263M was lost to smart contract bugs in DeFi in Q1 2025 alone — worst quarter since early 2023. Vault accounting errors and withdrawal race conditions are the specific patterns hitting yield protocols hardest.
>
> Cantina can help two ways: (1) a competition where 100+ researchers review your latest contracts in parallel, or (2) a managed bounty that catches issues continuously in production. Either way, 12,800+ researchers finding bugs before attackers do.
>
> Quick call to scope what makes sense for Kamino?

**Why this works:** Calls out the specific audit gap (18 months), names their auditor, flags the missing bounty, and references the exact vulnerability class relevant to yield vaults. Offers two concrete Cantina solutions, not a vague pitch.

---

## Example 3: EigenLayer (Infra/Restaking, $8B TVL)

**To:** Sreeram Kannan, Founder/CEO
**Channel:** Email (warm intro via Cantina researcher network)
**Signals used:** $8B TVL · Audited by Trail of Bits + Sigma Prime · Immunefi bounty at $2M · Very high velocity · Shipping new restaking features · Warm intro available

**Subject:** Cantina + EigenLayer: continuous security for the restaking layer

> Hi Sreeram,
>
> [Intro via mutual connection in Cantina's researcher network]
>
> EigenLayer's restaking architecture is one of the most complex smart contract systems in production — $8B in TVL across operator delegation, slashing conditions, and AVS integrations that compound interaction risks. Trail of Bits and Sigma Prime covered the core, but with the velocity of new AVS deployments and middleware being shipped, the gap between audited and live code keeps widening.
>
> We're seeing this pattern across infrastructure protocols: point-in-time audits can't keep pace with composability-driven development. Cantina's model — AI code analysis for continuous scanning, plus 12,800 researchers available for targeted competitions on new components — is built for exactly this.
>
> The Immunefi bounty at $2M is strong. But for $8B in TVL with new slashing and delegation logic going live monthly, a layered approach (audit + competition + continuous bounty + MDR) would close the gaps between audit cycles.
>
> Would 20 minutes make sense to discuss how we've structured this for Coinbase and Uniswap?
>
> Best,
> [Your name]

**Why this works:** References their exact architecture (operator delegation, slashing, AVS), names both auditors, acknowledges their existing bounty as "strong" (not dismissive), and frames Cantina as a layered addition — not a replacement. The warm intro makes this the highest-conversion message in the pipeline.

---

## How the Pipeline Scales This

In production, the outreach agent generates these automatically for every qualified lead:

1. **Input** — Enrichment data from the pipeline (TVL, audit status, category, velocity, persona, role, channel)
2. **System prompt** — Cantina positioning, category-specific risk hooks, tone rules, writing guidelines
3. **Output** — Personalized message body + subject line + list of signals used (stored for tracking)

The human reviews and approves before sending. The LLM does the heavy lifting on personalization; the human ensures quality and catches hallucinations. This is how you go from 3 outreach messages to 50/week without losing personalization quality.

All drafts are viewable in the UI before sending, stored in PostgreSQL, and tracked in HubSpot with outreach status.
