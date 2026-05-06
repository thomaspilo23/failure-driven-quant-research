# Feasibility Checklist

A structured set of tests to perform before committing to a research direction. Each section targets a failure mode that, in this research program, was not caught until after significant work had been invested. The purpose is to front-load the analyses that have historically been performed too late.

These are not theoretical exercises. Each test is grounded in a failure that occurred when the test was skipped.

---

## How to Use This

Work through the sections in order. Each section contains explicit stopping conditions. If a stopping condition is met, the correct action is to change the environment or the problem framing — not to attempt to engineer past the constraint. The stopping conditions are not conservative heuristics; they reflect cases where continued work produced nothing because the constraint was real.

---

## Section 1 — Required vs Achievable Performance

**What to compute:**

For any system that operates under a cost structure, compute the minimum performance required to break even. This is a function of the cost per action and the expected outcome magnitude per action — not a function of the model. Derive it analytically before training anything.

Separately, estimate the maximum performance achievable from the available signal space. Use the empirical distribution of the target variable over the evaluation period to establish an upper bound on predictability. This ceiling is bounded by the structure of the data, not by any specific model's capacity.

Compare the two numbers before proceeding.

**Why it matters:**

The required performance is fixed by the environment. The achievable ceiling is constrained by the signal space. If the required number exceeds the achievable ceiling, no model improvement closes the gap — because the ceiling is not a model property. Both numbers are computable before any model is trained.

This comparison was not performed before significant iteration in the mathematical limitation case. The required accuracy was 78%. The measured ceiling was 57%. The gap was discoverable in minutes; it was discovered after months.

**Common failure pattern:** Required performance is computed post-hoc, as an explanation for why iteration failed. By this point, the calculation confirms what iteration already established — at much higher cost. The calculation is treated as a diagnostic rather than as a prerequisite.

**What most approaches do instead:**
- Treat the gap between required and achievable performance as a modeling problem — assume better models extend the achievable ceiling.
- Add architectural complexity to close a gap that is bounded by the environment, not the model.
- Defer the feasibility calculation until performance fails to improve despite iteration.

**Stopping condition:**

If required performance exceeds the empirical upper bound of achievable performance from the signal space → stop. The environment does not permit a solution. Change the resolution, the cost structure, or the instrument class before proceeding.

---

## The Failure of Overlap Rule

If the minimum required performance — derived analytically from the environment's cost structure — exceeds the empirically observed maximum achievable performance from the available signal space, the problem is infeasible under current constraints.

This is not underfitting. A better model does not raise the achievable ceiling; the ceiling is bounded by the predictability of the target given the available inputs. No architectural change changes this.

This is not an optimization failure. No hyperparameter configuration closes the gap between required and achievable performance when that gap is a property of the environment.

This is an environmental impossibility. The constraint is in the ratio between action cost and signal magnitude at the given time resolution and instrument. The modeling pipeline is not the variable.

Required performance > achievable ceiling → change the environment. Stop optimizing the model.

---

## Section 2 — Environment Constraints

**What to compute:**

**Transaction cost structure:** Measure actual costs from the target execution environment directly. Published averages are a last resort — use them only when direct measurement is impossible. Measure per instrument, per session window, and per typical order size. Compute the ratio of cost to average outcome magnitude. The system is cost-sensitive when this ratio exceeds approximately 0.3; at that sensitivity, cost assumption errors have first-order effects on viability conclusions.

**Liquidity regime dependency:** Determine whether system outputs are uniform across liquidity conditions or concentrated in specific states. If outputs are concentrated in specific sessions or conditions, cost and fill assumptions must reflect those conditions precisely — not averaged across all conditions.

**Timeframe dependency:** Determine whether the signal exists only at a specific time resolution or persists across resolutions. If the signal is resolution-specific and that resolution carries prohibitive costs, it is not portable to a lower-cost resolution without re-verifying signal existence there.

**Why it matters:**

Environments differ in kind, not just in degree. The cost assumption failure reflected instrument-specific costs that differed from published averages by multiples — in opposite directions for different instruments in the same asset class. The rendering bottleneck reflected a resource cost that was negligible at prototype scale and prohibitive at training scale. Averaging across instruments or across scales produces conclusions that are wrong for every specific case.

**Common failure pattern:** Cost estimates from documentation or prior projects are applied to a new instrument or session without measurement. The system is evaluated against those estimates. Discrepancies are discovered when real execution data is collected — after evaluation is complete — requiring full re-evaluation.

**What most approaches do instead:**
- Carry forward cost estimates from prior work or from similar instruments without instrument-specific measurement.
- Evaluate session-dependent systems against aggregate costs rather than session-specific costs.
- Test at prototype scale and assume resource costs will scale linearly.

**Stopping condition:**

If measured costs differ from assumed costs by more than 2× in the direction that reduces expected outcome → re-evaluate viability with corrected costs before proceeding. If the system is not viable under corrected costs → stop.

If the signal is resolution-specific and the resolution is cost-prohibitive → do not attempt to apply it at a coarser resolution without first verifying signal existence at that resolution.

---

## Section 3 — Feature Information Test

**What to compute:**

**Pairwise correlation:** For any feature set with more than five members, compute the full pairwise correlation matrix before training. Identify pairs with correlation above 0.7 and clusters with mean pairwise correlation above 0.6. Features in a highly correlated cluster are transformations of the same underlying quantity — not independent sources of information.

**Marginal information contribution:** For each candidate feature, estimate the reduction in uncertainty about the target beyond what existing features already provide. Mutual information, partial correlation, or sequential feature selection with held-out validation each provide usable estimates. A feature with high individual predictive power but low marginal contribution is redundant.

**Transformation vs information:** Distinguish features that represent a genuinely different observable — a new measurement — from features that are mathematical transformations of observables already present. Momentum at a 14-period lag and momentum at a 28-period lag are transformations of the same observable. The label difference does not imply information difference.

**Why it matters:**

A model trained on a redundant feature set does not access more information than a model trained on a minimal non-redundant subset. It accesses the same information encoded more times. In the best case, this increases computational cost and regularization burden without improving performance. In practice, it provides additional surface area for learning the correlation structure of the training period — which does not persist out-of-sample.

In the feature overload case, the full feature set of thirty indicators had a median pairwise correlation above 0.85. Adding features beyond the first few decreased out-of-sample performance.

**Common failure pattern:** Features are added based on conceptual distinctiveness — the indicator measures something different — without computing marginal information contribution. The correlation matrix is computed after training, if at all, as a diagnostic rather than as a prerequisite.

**Stopping condition:**

If more than 60% of features have pairwise correlation above 0.7 with another feature → reduce to a minimal non-redundant set before training.

If a candidate feature's marginal information contribution is below the threshold required to influence model output at typical regularization strength → do not add it.

---

## Section 4 — Execution Reality

**What to compute:**

**Fill assumptions vs real fills:** Before evaluating any system against cost-adjusted outcomes, collect a sample of real fills from the target execution environment. Compare assumed fill prices against actual fills across representative instruments, sessions, and conditions. Compute the distribution of the gap — not just the mean.

**Session dependency:** Determine whether system outputs are concentrated in specific time windows. If so, the execution environment during those windows — costs, liquidity, fill rates — is the relevant measurement environment, not the aggregate environment.

**Instrument-specific behavior:** Do not assume that costs or fill characteristics transfer across instruments, even within the same asset class. Measure each instrument in the evaluation universe independently. Averaged costs produce conclusions that are wrong for every individual instrument.

**Why it matters:**

The execution layer is where the theoretical model encounters the real environment. Discrepancies between assumed and real execution conditions do not produce model errors — they produce evaluation errors that appear as model performance degradation until compared against real fills. In the cost assumption case, two instruments in the same asset class had real costs that differed from assumed costs by 3× to 6×, in opposite directions, with net effect on portfolio viability.

**Common failure pattern:** Real execution data is collected after evaluation is complete, to validate conclusions rather than to inform them. At this stage, discrepancies require re-evaluation from the beginning. The data that should have constrained the evaluation is used to explain why the evaluation was wrong.

**Stopping condition:**

If real fill data is unavailable for the target environment and instruments → treat all cost-sensitive conclusions as provisional. Do not advance to deployment decisions based on assumed costs.

If real costs change the viability conclusion for any instrument carrying more than 15% of expected output → re-evaluate the full portfolio with corrected costs before proceeding.

---

## Summary — Decision Rules

```
IF required performance > achievable performance ceiling
→ STOP. Change environment. (See: Failure of Overlap Rule)

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
