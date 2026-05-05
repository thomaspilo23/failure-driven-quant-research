# Failure-Driven Quant Research

> Failure-driven research for quantitative systems.

---

## What This Is

Most quant portfolios showcase wins. This one documents the failures — the dead ends, the subtle errors, and the structural lessons that only emerge when a system breaks in production.

The goal is not to prove competence through polished results. It is to demonstrate **research process, diagnostic rigor, and the ability to learn from failure at speed** — which is where durable edge actually comes from.

---

## Structure

```
/timeline          — Chronological map of research phases and pivots
/case-studies      — Sanitized failure post-mortems, each with root cause analysis
/framework         — The decision-making and evaluation framework that emerged from failure
/tools             — Lightweight diagnostic and analysis utilities
/assets            — Charts, diagrams, supporting visuals
```

---

## Case Studies

| Study | Domain | Core Failure |
|-------|--------|--------------|
| [indicator-overload](case-studies/indicator-overload/) | Signal design | Too many correlated features, no signal |
| [intraday-friction](case-studies/intraday-friction/) | Execution modeling | Ignored spread + slippage, backtest ≠ live |
| [image-based-ml-compute-failure](case-studies/image-based-ml-compute-failure/) | ML architecture | GPU memory constraints broke training pipeline |
| [tabular-model-selection-failure](case-studies/tabular-model-selection-failure/) | Model selection | Overfit to validation set, GBDT vs NN choice |
| [regime-calendar-dependency](case-studies/regime-calendar-dependency/) | Regime detection | Model trained on one macro regime, calendar artifacts |

---

## Framework

The [framework](framework/) section distills the evaluation and decision-making rules that survived contact with these failures: what to test first, how to diagnose a broken backtest, when to kill a strategy.

---

## Principles

- No data snooping. Walk-forward only.
- Failure is documented, not hidden.
- Synthetic and anonymized data only — no live system details.
- Every case study has a root cause, not just a symptom.

---

*Research ongoing. Work in progress.*
