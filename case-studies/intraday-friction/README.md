# Case Study: Intraday Friction

**Status:** Post-mortem complete  
**Domain:** Execution modeling  
**Outcome:** Strategy reparametrized

---

## Summary

A short-term intraday strategy showed consistent positive expectancy in backtests. In live simulation, it was unprofitable. The gap was entirely explained by transaction costs: bid-ask spread, slippage on market orders, and per-trade commission were not modeled in the backtest.

---

## Timeline

- **Phase 1:** Backtest on mid-price (no spread). Looked profitable at 0.3R/trade average
- **Phase 2:** Added flat per-trade commission. Still profitable
- **Phase 3:** Added realistic spread model. Edge dropped to near zero
- **Phase 4:** Added slippage on entry/exit. Strategy went negative expectancy
- **Phase 5:** Recalculated required gross edge per trade to survive full friction — target was 3x higher than backtest showed

---

## Root Cause

**Mid-price backtesting without friction modeling for a high-frequency strategy.**

At short holding periods, transaction costs consume a disproportionate share of gross PnL. A strategy with 0.3R gross edge per trade cannot survive 0.25R in friction costs.

---

## Lessons

1. Always model spread + slippage + commission from day one — not as an afterthought
2. Friction scales inversely with holding period. Short-term strategies have almost no margin for error
3. Gross edge required = (friction cost) × (safety factor ≥ 3×)
4. Mid-price backtesting is only valid for longer-horizon strategies with large R multiples

---

## What Changed

Added a friction model as a mandatory pre-check before any backtest is considered valid. Minimum gross edge threshold enforced programmatically.

---

*Data in this case study is synthetic. No live system details are included.*
