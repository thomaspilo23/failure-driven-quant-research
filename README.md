# Failure-Driven Quant Research

---

## What This Is

Most research portfolios document what worked. This one documents what didn't — and why that distinction matters.

Systematic quantitative research fails in specific, instructive ways. Signals that look real in backtests disappear under live costs. Execution systems pass every test and produce zero actions. Mathematically sound approaches prove infeasible before any model is trained. Infrastructure assumptions that hold in development break in production. Each failure type has a structure, and that structure is learnable.

The goal of this repository is not to demonstrate correctness through polished results. It is to demonstrate research process: how problems are diagnosed, how assumptions are tested, how direction changes when evidence demands it. These are the competencies that determine whether a systematic research program produces durable results — and they are rarely visible in portfolios that show only the outcomes.

All case studies are abstracted and sanitized. No proprietary logic, implementation details, or live system specifics are included.

---

## Case Studies

Each case study is a post-mortem of a failure that changed the direction of the research. The emphasis is on the investigation and the realization — not the fix.

| | Case | Type | One-Line Summary |
|---|---|---|---|
| [→](case-studies/model-that-never-traded/) | The Model That Never Traded | Execution | A trained model produced zero actions for months because of a one-line index error at the integration boundary. |
| [→](case-studies/when-the-math-says-no/) | When the Math Says No | Mathematical Limitation | An arithmetic audit proved an entire research direction infeasible before any model could make it work. |
| [→](case-studies/four-experiments-zero-progress/) | Four Experiments. Zero Progress. One Correct Answer. | Model Failure | After four engineering solutions failed identically, investigation revealed the problem was structural — not fixable from inside the system. |
| [→](case-studies/the-wrong-account/) | The Wrong Account | Execution | A system test succeeded in every measurable way while routing actions to the wrong destination. |
| [→](case-studies/the-cost-assumption/) | The Cost Assumption | Data Failure | A strategy with a real edge appeared viable against estimated costs and unviable against measured ones. |
| [→](case-studies/rendering-bottleneck/) | The Rendering Bottleneck | Infrastructure | A theoretically sound visual encoding approach failed because generating the representation consumed the compute budget before the model could use it. |
| [→](case-studies/more-features-less-signal/) | More Features, Less Signal | Model Failure | A feature set assembled from thirty well-understood indicators carried less information than a set of five. |

---

## Structure

```
/case-studies      — Failure post-mortems: context, breakdown, investigation, realization
/framework         — Decision-making and evaluation principles (planned)
/timeline          — Chronological map of research phases and pivots (planned)
/tools             — Diagnostic and analysis utilities (planned)
/assets            — Supporting visuals (planned)
```

---

## Principles

- Every case study identifies a root cause, not just a symptom.
- Failures are documented with the same rigor applied to successes.
- All material is abstracted — no live system details, no proprietary implementation.

---

*Research ongoing. Work in progress.*
