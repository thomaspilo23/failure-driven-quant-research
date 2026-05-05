# Kill Criteria

Conditions under which continued work on a research direction should stop. This document addresses a specific failure mode: continuing to invest resources in a direction that has already produced enough evidence to justify stopping, because the stopping signal was not recognized as such.

This is not a pessimistic document. Identifying a direction as infeasible — and doing so with precision, at low cost — is a research output. It closes a hypothesis. The failure is not in the direction being wrong; it is in reaching that conclusion slowly and expensively rather than quickly and cheaply.

---

## Section 1 — Iteration Failure Signals

These signals indicate that continued iteration on the current approach is not converging toward a solution.

**Multiple approaches, zero improvement**

When three or more independent approaches — differing in mechanism, not just in parameters — produce identical results on the target problem, the prior probability that a new approach would succeed drops sharply. The word "independent" is critical: approaches that differ only in configuration or threshold are not independent. Approaches that differ in their fundamental mechanism — what they do to the system, not how much — are independent.

Three independent failures is a threshold, not a rule. Two independent failures with identical failure signatures are sufficient to raise the structural hypothesis.

**Identical failure signature across variants**

When multiple approaches fail in the same way — producing the same type of outcome in the same conditions, not just similar magnitude of failure — the failure is not random and not approach-specific. A failure pattern that persists across mechanically different interventions is carrying information about the problem structure, not about the approaches.

The four-experiment case produced zero improvement across four mechanically distinct interventions: a soft multiplier, a hard constraint, a pre-entry filter, and a real-time diagnostic. The identical outcome across all four was not coincidence. It was the signal that the failure was structural.

**Improvement in controlled evaluation but not in target conditions**

When a modification produces measurable improvement in the evaluation environment but not in the conditions it was designed to address, the modification is not addressing the root cause. It is changing system behavior in the evaluation environment while the actual failure mechanism remains active in the target conditions.

This signal is particularly important because it mimics progress. Metrics improve. The mechanism is not fixed.

---

## Section 2 — Structural Signals

These signals indicate that the failure is not in any component or configuration, but in the architecture or environment — and is therefore not addressable through continued iteration on the current layer.

**Behavior inversion under specific conditions**

When a system produces favorable outcomes in condition A and unfavorable outcomes in condition B, and the mechanism that produces the outcomes in both conditions is the same, the failure in condition B is not a malfunction — it is the correct output of the mechanism operating in an incompatible environment. Any intervention that suppresses the mechanism in condition B will suppress it proportionally in condition A. The mechanism cannot be made to work in condition B without changing what it does in condition A.

This is the defining characteristic of a structural failure. The mechanism is not wrong. The environment is incompatible.

**Dependency on conditions not detectable by the system**

When a failure pattern is conditionally predictable from external information that the system does not have access to — information that could label the failure conditions in advance — but is not predictable from information the system does have access to, the failure cannot be addressed from inside the system. The discriminating signal is external to the system's observable space.

**Expected outcome disappears under realistic costs**

When the positive expected outcome of a research direction is present under idealized conditions but absent or negative under realistic costs, the direction is cost-sensitive in a way that makes the idealized result not useful. The realistic conditions are the relevant conditions. If the direction cannot produce positive expected outcome under the conditions it will actually operate in, the result under idealized conditions is not a result.

---

## Section 3 — False Progress Signals

These signals create the appearance of progress without movement toward a solution. Recognizing them prevents resources from being allocated to activities that feel productive but are not.

**Metric improvement without outcome change**

A metric that improves without changing the outcome being optimized is not measuring the right thing, or is being optimized in isolation from the outcome. This appears frequently in model development: training metrics improve while target-condition performance remains flat. The investigation focuses on the improving metric, which is the wrong thing to investigate.

**Complexity increase without explanatory power**

When adding complexity to a system — more features, more components, more parameters, more conditional logic — does not increase the system's ability to explain or predict the target outcome, the complexity is not contributing to the research direction. It is increasing the surface area for overfitting, increasing computational cost, and making the system harder to diagnose without reducing the failure.

**Optimization without new information**

When an optimization process runs to convergence but no new information about the failure mechanism has been generated — when the result of the optimization is a different configuration that produces the same failure — the optimization was not the right operation to perform. Optimizing a configuration that is structurally wrong produces a different wrong configuration, not a correct one.

The appropriate question before optimization is: does the current architecture have the capacity to solve this problem at any configuration? If the answer is no, optimization will not find one.

---

## Section 4 — Decision Rules

Hard rules derived from the failure patterns in this research program. Each rule has a condition and a prescribed action. The actions are binary: continue or stop. There is no "reduce investment and continue" option — that path typically produces the same conclusion at higher total cost.

---

**Rule 1 — Structural Failure Threshold**

*If three or more independent approaches, differing in fundamental mechanism, fail on the same target problem with identical failure signatures → classify the failure as structural.*

*Action: Stop iterating on the current layer. Evaluate whether the problem is addressable from a different architectural layer or through an orthogonal mechanism. Do not design a fourth approach on the same layer.*

---

**Rule 2 — Mathematical Infeasibility**

*If the analytically derived required performance exceeds the empirically derived achievable performance ceiling from the available signal space → the problem is not solvable within the current environment.*

*Action: Stop. The constraint is environmental. Changing the model does not change the constraint. The only productive actions are: change the environment (resolution, cost structure, instrument), or close the hypothesis.*

---

**Rule 3 — Cost Sensitivity Breach**

*If the expected outcome of a research direction is positive under assumed costs and negative or zero under measured costs, and the cost difference reflects the target environment rather than measurement error → the direction is not viable.*

*Action: Stop. Do not attempt to engineer a solution within the cost-prohibitive environment. Evaluate whether the cost structure is negotiable (instrument choice, timing, execution method) or whether the direction must be abandoned.*

---

**Rule 4 — Behavior Inversion**

*If a system produces the mechanism's intended outcome in baseline conditions and the opposite outcome in a specific identifiable environment, and the mechanism is the same in both cases → the failure in the specific environment is structural.*

*Action: Accept the residual. Do not attempt to fix the mechanism from inside. If the specific environment can be addressed, address it through an orthogonal mechanism that operates independently of the primary mechanism — one that does not interact with it in baseline conditions.*

---

**Rule 5 — Integration Validation Prerequisite**

*If a component has not been tested end-to-end with its downstream consumer under conditions that exercise the interface → do not treat the component as validated.*

*Action: Do not advance a component to production readiness without an integration test that asserts the system's behavioral output — not just its operational output — against a known expected result.*

---

**Rule 6 — Metric Decoupling**

*If a measured metric is improving while the outcome that metric is intended to predict is not changing → the metric is not measuring the right thing, or is being measured in the wrong context.*

*Action: Stop optimizing the metric. Identify what is causing the decoupling before continuing.*

---

## Using the Rules

The rules are not a replacement for judgment. They are a way to force the structural question earlier in the research process, before the cost of continued iteration has compounded. The structural question is: is this a problem that continued work on this approach can solve, or is the constraint elsewhere?

That question can usually be answered faster than the iteration that would eventually produce the same answer.
