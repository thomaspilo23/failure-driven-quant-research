# Feasibility Checklist

A structured set of tests to perform before committing to a research direction. Each section targets a failure mode that, in this research program, was not caught until after significant work had been invested. The purpose is to front-load the analyses that have historically been performed too late.

These are not theoretical exercises. Each test is grounded in a failure that occurred when the test was skipped.

---

## How to Use This

Work through the sections in order. Each section contains explicit stopping conditions. If a stopping condition is met, the correct action is to change the environment or the problem framing — not to proceed and attempt to engineer past the constraint. The stopping conditions are not conservative heuristics; they reflect cases where continued work produced nothing because the constraint was real.

---

## Section 1 — Required vs Achievable Performance

**What to compute:**

For any system that operates under a cost structure, compute the minimum performance required to break even. This is a function of the cost per action and the expected outcome magnitude per action, not a function of the model. Derive it analytically before training anything.

Separately, estimate the maximum performance achievable from the available signal space. For price-derived signals, this ceiling is bounded by the predictability of the target at the given time resolution. Use the empirical distribution of the target over the evaluation period to establish an upper bound.

Compare the two numbers.

**Why it matters:**

The required performance is fixed by the environment. The achievable performance is constrained by the signal space. If the required number exceeds the achievable ceiling, no model improvement closes the gap — because the ceiling is not a model property.

This comparison was not performed before significant iteration in the mathematical limitation case. The required accuracy was 78%. The measured ceiling was 57%. The gap was discoverable in minutes; it was discovered after months.

**What failure looks like:**

Required performance is computed only after model development, once iteration has not produced improvement. At that point the calculation confirms what the iteration already established — but at much higher cost.

**Stopping condition:**

If required performance exceeds the empirical upper bound of achievable performance from the signal space → stop. The environment does not permit a solution. Change the resolution, the cost structure, or the instrument class before proceeding.

---

## Section 2 — Environment Constraints

**What to compute:**

**Transaction cost structure:** Measure actual costs from the target execution environment directly. Do not use published averages unless direct measurement is impossible. Measure per instrument, per session window, and per typical order size. Compute the ratio of cost to average outcome magnitude. The system is cost-sensitive when this ratio exceeds approximately 0.3 — meaning costs consume more than 30% of the gross expected outcome. At this sensitivity level, cost errors have first-order effects on viability conclusions.

**Liquidity regime dependency:** Determine whether the system's outputs are uniform across liquidity conditions or whether they depend on specific liquidity states. If outputs are concentrated in specific sessions or market conditions, the cost and fill assumptions must reflect those conditions, not average conditions.

**Timeframe dependency:** Determine whether the signal decays with time resolution — whether the pattern that generates the signal exists only at a specific resolution or whether it persists across resolutions. If the pattern is resolution-specific and that resolution carries prohibitive costs, the signal is not portable to a more favorable cost environment.

**Why it matters:**

Environments differ not as a matter of degree but in kind. The cost structure of one instrument or session is not representative of another. The rendering bottleneck failure reflected a resource cost that was negligible at prototype scale and prohibitive at training scale. The cost assumption failure reflected instrument-specific costs that differed from averages by multiples.

**What failure looks like:**

Cost assumptions drawn from published sources or prior projects are carried forward without measurement. The system is evaluated against those assumptions and conclusions are drawn. The conclusions are invalidated when real execution data is compared against the assumptions.

**Stopping condition:**

If measured costs differ from assumed costs by more than 2× in the direction that reduces expected outcome → re-evaluate viability with corrected costs before continuing. If the system is not viable under corrected costs → stop.

If the signal is resolution-specific and the resolution is cost-prohibitive → do not attempt to apply it at a coarser resolution without re-verifying signal existence at that resolution.

---

## Section 3 — Feature Information Test

**What to compute:**

**Pairwise correlation:** For any feature set with more than five members, compute the full pairwise correlation matrix before training. Identify pairs with correlation above 0.7 and clusters with mean pairwise correlation above 0.6. Features in a highly correlated cluster are not independent sources of information — they are transformations of the same underlying quantity.

**Marginal information contribution:** For each candidate feature, estimate the reduction in uncertainty about the target that the feature provides beyond what existing features already provide. Mutual information, partial correlation, or sequential feature selection with held-out validation each provide usable estimates. A feature with high individual predictive power but low marginal contribution is redundant.

**Transformation vs information:** Distinguish between features that represent a genuinely different observable — a new measurement of the system — and features that are mathematical transformations of observables already present in the feature set. Momentum at a 14-period lag and momentum at a 28-period lag are transformations of the same observable, not independent observations. The label difference does not imply information difference.

**Why it matters:**

A model trained on a redundant feature set does not have access to more information than a model trained on a minimal non-redundant subset. It has access to the same information encoded more times. In the best case, this increases computational cost and regularization burden without improving performance. In the common case, it provides additional surface area for overfitting to the correlation structure of the training period.

In the feature overload case, the full feature set of thirty indicators had a median pairwise correlation above 0.85. Adding features beyond the first few decreased out-of-sample performance rather than increasing it.

**What failure looks like:**

Feature set grows incrementally, with each addition justified by what the indicator measures in isolation rather than by what it contributes given existing features. Model complexity increases. Training performance remains stable or improves. Out-of-sample performance does not improve or degrades.

**Stopping condition:**

If more than 60% of features have pairwise correlation above 0.7 with another feature → reduce to a minimal non-redundant set before proceeding. Do not train on the full set.

If a candidate feature has marginal information contribution below a threshold that would be required for it to influence model output at typical regularization strength → do not add it.

---

## Section 4 — Execution Reality

**What to compute:**

**Fill assumptions vs real fills:** Before evaluating any system against cost-adjusted outcomes, collect a sample of real fills from the target execution environment. Compare assumed fill prices against actual fill prices across representative instruments, sessions, and conditions. Compute the distribution of the gap, not just the mean.

**Session dependency:** Determine whether system outputs are concentrated in specific time windows. If so, the execution environment during those windows — costs, liquidity, fill rates — is the relevant measurement environment, not the aggregate environment.

**Instrument-specific behavior:** Do not assume that costs or fill characteristics transfer across instruments, even within the same asset class. Measure each instrument in the evaluation universe independently. Averaged costs across instruments produce conclusions that are wrong for every individual instrument in opposite directions.

**Why it matters:**

The execution layer is where the theoretical model encounters the real environment. Discrepancies between assumed and real execution conditions do not produce model errors — they produce evaluation errors that look like model performance until compared against real execution data. In the cost assumption case, two instruments in the same asset class had real costs that differed from assumed costs by 3× to 6×, in opposite directions, with net effect on the portfolio viability conclusion.

**What failure looks like:**

Real execution data is collected after evaluation is complete, to validate results rather than to inform them. Discrepancies discovered at this stage require re-evaluation from the beginning.

**Stopping condition:**

If real fill data is not available for the target environment and instruments → treat all cost-sensitive conclusions as provisional until it is collected. Do not advance to deployment decisions based on assumed costs.

If real costs change the viability conclusion for any instrument carrying more than 15% of expected output → re-evaluate the portfolio with corrected costs before proceeding.

---

## Summary — Decision Rules

```
IF required performance > achievable performance ceiling
→ STOP. Change environment.

IF measured costs differ from assumed costs by >2× (adverse direction)
→ STOP. Re-evaluate with corrected costs.

IF system is not viable under corrected costs
→ STOP.

IF >60% of features have pairwise correlation >0.7
→ STOP. Reduce to non-redundant set before training.

IF signal is resolution-specific AND that resolution is cost-prohibitive
→ STOP. Do not assume portability to other resolutions.

IF real execution data is unavailable
→ HOLD all viability conclusions. Do not advance to deployment.

IF none of the above apply
→ PROCEED to signal and model development.
```
