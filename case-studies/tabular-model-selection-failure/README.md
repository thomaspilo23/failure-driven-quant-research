# Case Study: Tabular Model Selection Failure

**Status:** Post-mortem complete  
**Domain:** Model selection / ML  
**Outcome:** Model selection process rebuilt

---

## Summary

After switching to tabular features, a neural network (MLP) was selected over gradient boosting (GBDT) based on validation set performance. In walk-forward testing, the GBDT consistently outperformed the MLP. The root cause was implicit overfitting to the validation set during model selection — the MLP had more hyperparameters and found a better local fit to the specific validation window.

---

## Timeline

- **Phase 1:** Trained MLP and GBDT on same feature set
- **Phase 2:** Compared on fixed validation split — MLP won (Sharpe 1.4 vs 1.1)
- **Phase 3:** Selected MLP, ran walk-forward. Performance degraded
- **Phase 4:** Re-ran walk-forward on GBDT — consistently better across folds
- **Phase 5:** Root cause identified: single validation split created selection bias

---

## Root Cause

**Model selection on a single validation split with a higher-capacity model.**

The MLP's higher capacity allowed it to fit the specific validation window better without generalizing. The GBDT's inductive bias (tree structure, feature interactions) was better suited to the data distribution.

---

## Lessons

1. Model selection must use multiple out-of-sample windows (k-fold or anchored walk-forward), not a single split
2. Higher validation performance with a higher-capacity model is a warning sign, not a win
3. For financial tabular data, GBDT is a strong default — neural networks require strong justification
4. Selection bias compounds: every decision made on validation data leaks information

---

## What Changed

Adopted a mandatory multi-fold selection protocol. GBDT is now the default; neural networks require a pre-registered hypothesis before testing.

---

*Data in this case study is synthetic. No live system details are included.*
