# Case Study: Indicator Overload

**Status:** Post-mortem complete  
**Domain:** Signal design  
**Outcome:** Strategy discarded

---

## Summary

A signal-generation system was built using 20+ technical indicators across multiple timeframes. Backtest performance looked promising. Live performance was flat. The root cause was redundancy: most indicators were mathematical transformations of the same price series, providing near-zero marginal information.

---

## Timeline

- **Phase 1:** Built feature library with 20+ indicators (momentum, volatility, trend, oscillators)
- **Phase 2:** Ran optimization over indicator combinations — found strong in-sample fits
- **Phase 3:** Out-of-sample collapse. Sharpe dropped from 1.8 to 0.2
- **Phase 4:** Correlation analysis revealed ~80% of features had pairwise correlation > 0.85

---

## Root Cause

**Signal redundancy masquerading as signal richness.**

Adding more indicators did not add information — it added noise and overfitting surface. The optimization found spurious combinations that fit historical noise rather than any repeatable structure.

---

## Lessons

1. Measure marginal information gain before adding any feature (mutual information, partial correlation)
2. PCA or feature importance pre-screening is not optional on high-dimensional signal sets
3. In-sample Sharpe > 1.5 on a large feature set is a red flag, not a green light
4. Fewer, orthogonal signals outperform large correlated feature sets

---

## What Changed

Rebuilt signal layer from scratch using a maximum of 5 features with verified low pairwise correlation. Applied walk-forward selection only.

---

*Data in this case study is synthetic. No live system details are included.*
