# Case Study: Regime-Calendar Dependency

**Status:** Post-mortem complete  
**Domain:** Regime detection / feature engineering  
**Outcome:** Regime model rebuilt

---

## Summary

A regime-detection model was trained on data from a single macroeconomic environment (low-volatility, trending). It performed well in-sample and on a held-out test set from the same period. When deployed into a different macro environment (high-volatility, mean-reverting), the regime labels became unreliable and the downstream strategy degraded. A secondary issue was discovered: the model had implicitly learned calendar patterns (day-of-week, month-end effects) that were spurious correlations.

---

## Timeline

- **Phase 1:** Trained regime classifier on 2 years of data from a single macro environment
- **Phase 2:** High accuracy on held-out test set from same period
- **Phase 3:** Deployed into new market environment — regime labels unstable
- **Phase 4:** Audit revealed model had high feature importance on calendar-derived features
- **Phase 5:** Calendar features removed, model retrained across multiple macro environments

---

## Root Cause

**Training distribution mismatch + spurious calendar feature leakage.**

The model learned to classify regimes using calendar artifacts that happened to correlate with regimes in the training period. When the macro environment changed, both the regime structure and the calendar correlations broke simultaneously.

---

## Lessons

1. Regime models must be trained across multiple distinct macro environments, not one
2. Calendar features (day-of-week, month, quarter) are high-risk — validate that any learned correlation is causal, not coincidental
3. Feature importance audits are mandatory before any model goes to walk-forward
4. A model that works in one regime and fails in another has not learned regime structure — it has learned one regime

---

## What Changed

Regime model retrained across multiple environment samples. Calendar features removed entirely pending causal validation. Added an environment-shift detection check before deploying any regime model.

---

*Data in this case study is synthetic. No live system details are included.*
