# Instrumentation Plan: 30-Day Tracking

## If We Book 10 Discovery Calls

### Core Metrics

| Metric | Target | What It Measures | Tool |
|--------|--------|-----------------|------|
| **Booking rate** | >15% | Outreach → booked call conversion | HubSpot sequences |
| **Hypothesis confirmation rate** | >60% | % of calls where prospect confirms AI code security is a problem | Post-call form → PostgreSQL |
| **Pain severity** | Avg ≥7/10 | Self-reported urgency (1-10) | Post-call form |
| **Using AI for contracts** | Track % | % of prospects confirmed using Copilot/Cursor/Claude for Solidity/Rust | Post-call form |
| **Audit bottleneck confirmed** | >50% | % who say their audit process can't keep up with shipping speed | Post-call form |
| **ICP signal accuracy** | >70% | Did our enrichment signals (TVL, audit status, velocity) match reality? | Signal vs. reality comparison |
| **Pipeline velocity** | <14 days | First outreach → booked call elapsed time | HubSpot timestamps |
| **Next-step conversion** | >50% | % of calls advancing to scoping/demo/CISO intro | HubSpot deal stages |
| **Channel attribution** | Tracked | Which channel (Twitter DM, email, warm intro) drove the booking | HubSpot source tracking |

### Post-Call Structured Form

After every discovery call, the caller fills out (2 minutes):

```
Protocol: _______________
Call date: _______________
Prospect name + role: _______________

HYPOTHESIS VALIDATION:
- Pain confirmed? [Y/N]
- Pain severity (1-10): ___
- Using AI for smart contracts? [Y/N]
- Which tools? [Copilot / Cursor / Claude Code / Remix AI / Other]
- Current audit provider: _______________
- Audit keeping pace with shipping? [Y/N]
- Had exploit or near-miss? [Y/N]

ICP VALIDATION:
- Did our enrichment data match reality? [Y/N]
- What was different? _______________

CANTINA FIT:
- Interested in: [Security review / Bug bounty / Competition / MDR / AI analyzer]
- Current bounty platform: _______________
- Willing to switch/add Cantina? [Y/N]

HYPOTHESIS B SIGNAL:
- Tool consolidation mentioned? [Y/N]
- Notes: _______________

OUTCOME:
- Top objection: _______________
- Next step: [Scoping call / Demo / CISO intro / Referral / No next step]
- Deal potential: [High / Medium / Low / None]
- Surprise insight: _______________
```

This form maps directly to the `discovery_calls` table in the database.

### Day 30 Decision Framework

**GREEN — Scale (6+ of 10 confirm)**
- 6+ calls confirm AI code security gaps
- Pain severity ≥7/10 average
- 3+ convert to next step
- **Action:** Double down. Automate full pipeline on AWS Lambda. Scale from 8 targets to 50+/week. Build event-triggered outreach system.

**YELLOW — Refine (4-5 confirm)**
- 4-5 confirm but severity is moderate (5-6/10)
- "It's on our roadmap but not urgent"
- **Action:** Tighten ICP. Maybe pain is sharper for specific protocol categories (bridges > DEXs?) or stages (pre-launch > established). A/B test messaging angles.

**RED — Pivot (fewer than 4 confirm)**
- Common objection: "We handle this internally" or "Our current auditors are fine"
- **Action:** Hypothesis A as framed doesn't resonate. Investigate: wrong messaging, wrong audience, or wrong hypothesis? Run 5 calls positioned around tool consolidation (Hypothesis B) to compare.

### Scoring Model Recalibration

After 10 calls, the feedback data recalibrates scoring weights:

```python
# Example: if audit_status predicted conversion 3x better than tvl
# Before: tvl=30, audit=25, velocity=20, funding=15, reach=10
# After:  tvl=20, audit=35, velocity=20, funding=15, reach=10
```

The recalibration runs as a Python script (weekly cron) that:
1. Queries `discovery_calls` for confirmed pain + next-step conversion data
2. Correlates with `lead_scores` factor breakdown
3. Computes which factors best predicted positive outcomes
4. Updates `config/scoring_weights.json`
5. Logs the change in `scoring_model_versions` table
6. Alerts via Slack

### What "Confirmed Signal" Looks Like

The strongest validation would be a prospect saying some version of:

> "Yes, our devs are using Copilot/Cursor to write Solidity. Yes, we've found bugs in AI-generated contract code that our auditors missed. Yes, we need continuous security coverage, not just point-in-time audits. What can Cantina do?"

Even partial confirmation is valuable:
- "We're not using AI for contracts yet, BUT our audit process is already too slow" → velocity is the pain, not AI specifically
- "We use Immunefi but the quality of submissions is low" → Cantina's triage quality is the differentiator
- "We need an audit but can't get on anyone's calendar for 3 months" → supply/demand gap is the pain

Track the nuance. The pipeline gets smarter with every call.
