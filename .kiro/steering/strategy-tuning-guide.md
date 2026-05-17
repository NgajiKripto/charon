# Strategy Tuning Guide

This document provides recommended settings for each Charon strategy to improve profitability and reduce risk. Default settings are intentionally loose for testing; production usage requires tightening filters significantly.

## Key Principles

1. **Filter aggressively** — 90%+ of Pump.fun tokens go to zero. Only trade the top 5-10%.
2. **Cut losses fast** — Meme coins dump hard and fast. Small SL = small loss per trade.
3. **Take profit realistically** — +30-40% is achievable; +100% is rare and often reverses before hit.
4. **Limit exposure** — Never risk more than 10% of capital at once across all open positions.
5. **Time-box positions** — Meme coin momentum is short-lived (1-4 hours max).

## Recommended Sniper Settings (Production)

```bash
/stratset sniper min_mcap_usd 30000
/stratset sniper max_mcap_usd 100000
/stratset sniper min_holders 200
/stratset sniper max_top20_holder_percent 50
/stratset sniper min_fee_claim_sol 2
/stratset sniper min_gmgn_total_fee_sol 5
/stratset sniper trending_min_volume_usd 10000
/stratset sniper trending_min_swaps 50
/stratset sniper trending_max_rug_ratio 0.15
/stratset sniper trending_max_bundler_rate 0.3
/stratset sniper tp_percent 35
/stratset sniper sl_percent -15
/stratset sniper trailing_enabled true
/stratset sniper trailing_percent 12
/stratset sniper max_hold_ms 3600000
/stratset sniper position_size_sol 0.05
/stratset sniper max_open_positions 2
/stratset sniper llm_min_confidence 75
/stratset sniper min_source_count 2
/stratset sniper require_fee_claim true
```

## Why Default Settings Cause Losses

| Default Setting | Problem | Fix |
|-----------------|---------|-----|
| `min_mcap_usd: 7000` | Ultra-low liquidity, 10-30% slippage on entry | Raise to 30,000+ |
| `min_holders: 0` | Tokens with 5 holders (instant rug) can pass | Require 200+ |
| `max_top20_holder_percent: 100` | Single whale holding 90% can dump anytime | Cap at 50% |
| `tp_percent: 50` | Rarely reached; price reverses at +30-40% | Lower to 35% |
| `sl_percent: -25` | Too wide; real loss = -25% + slippage = -35% | Tighten to -15% |
| `trailing_percent: 20` | Meme dumps gap -30% in one block, trailing never triggers in time | Reduce to 12% |
| `max_hold_ms: 0` | Positions stuck forever in sideways market | Set to 1-2 hours |
| `llm_min_confidence: 50` | 50% = coin flip; LLM approves mediocre entries | Raise to 75% |
| `trending_min_volume_usd: 0` | $50 volume tokens pass as "trending" | Require $10K+ |

## Position Sizing Guidelines

| Capital | position_size_sol | max_open_positions | Max exposure |
|---------|-------------------|--------------------|--------------|
| < 2 SOL | 0.03 | 1 | 1.5% of capital |
| 2-5 SOL | 0.05 | 2 | 2-5% of capital |
| 5-20 SOL | 0.1 | 2-3 | 3-6% of capital |
| 20+ SOL | 0.2-0.5 | 3-5 | 5-10% of capital |

## Workflow: Dry Run First

1. Start with `TRADING_MODE=dry_run` for 3-7 days
2. Run `/learn 24h` daily to review performance
3. Review `/lessons` to see what the bot learned
4. Adjust settings based on win rate and PnL
5. Only switch to `confirm` mode after consistent dry-run profits
6. Only switch to `live` after comfortable with `confirm` results
