# Kill Criteria

Conditions under which continued work on a research direction must stop. This document addresses a specific failure mode: continuing to invest resources in a direction that has already produced sufficient evidence to justify stopping, because the stopping signal was not recognized as such.

Identifying a direction as infeasible — precisely, at low cost — is a research output. It closes a hypothesis. The failure is not in the direction being wrong; it is in reaching that conclusion slowly and expensively rather than quickly and cheaply.

---

## Section 1 — Iteration Failure Signals

Signals indicating that continued iteration on the current approach is not converging toward a solution.

**Multiple approaches, zero improvement**

When three or more independent approaches — differing in mechanism, not just in parameters — produce identical results on the target problem, the prior probability that a new approach would succeed drops sharply. "Independent" means differing in fundamental mechanism: what the intervention does to the system, not how much. Approaches that differ only in configuration or threshold are not independent.

Three independent failures is a threshold, not a rule. Two independent failures with identical failure signatures are sufficient to raise the structural hypothesis.

**Identical failure signature across variants**

When multiple approaches fail in the same way — producing the same type of outcome in the same conditions, not merely similar magnitude — the failure is not approach-specific. A failure pattern that persists across mechanically different interventions carries information about the problem structure, not about the approaches.

The four-experiment case produced zero improvement across four mechanically distinct interventions: a soft multiplier, a hard constraint, a pre-entry filter, and a real-time diagnostic. The identical outcome was the signal that the failure was structural.

**Common failure pattern:** A third approach is designed to be more sophisticated than the first two. It addresses a different implementation detail of the same underlying mechanism. The failure pattern is identical to the first two. The conclusion drawn is that the fourth approach must address yet another implementation detail rather than that the mechanism itself is the wrong layer to intervene on.

**Improvement in controlled evaluation but not in target conditions**

When a modification produces measurable improvement in the evaluation environment but not in the conditions it was designed to address, it is not addressing the root cause. It is changing system behavior in the evaluation environment while the actual failure mechanism remains active in the target conditions.

This signal mimics progress. Metrics improve. The mechanism is not fixed.

---

## The Escalation Rule

When multiple independent approaches fail identically, confidence in the implementation must decrease more slowly than confidence in the problem formulation.

The natural update upon a second or third failure is to design a better implementation. The correct update is to increase suspicion that the approaches share an assumption — at the level of the problem formulation, not the implementation — that makes all of them fail. An implementation error produces a specific failure signature. An assumption error produces an identical failure signature across every implementation that inherits the assumption.

If three implementations differ in mechanism and produce the same failure pattern, they are either sharing a flawed assumption about the problem formulation, or operating in an environment that makes the target behavior impossible at this layer. Neither diagnosis is addressable by designing a more careful implementation on the same layer.

Escalation means: move the question up a level. Not "what is wrong with this approach?" but "what assumption do all approaches share that makes them fail the same way?"

The answer to that question identifies either the structural constraint or the incorrect framing — both of which determine whether to change layer, change environment, or close the hypothesis.

---

## Section 2 — Structural Signals

Signals indicating that the failure is not in any component or configuration, but in the architecture or environment — and is not addressable through continued iteration on the current layer.

**Behavior inversion under specific conditions**

When a system produces favorable outcomes in condition A and unfavorable outcomes in condition B, and the mechanism is the same in both cases, the failure in condition B is the correct output of the mechanism operating in an incompatible environment. Any intervention that suppresses the mechanism in condition B suppresses it proportionally in condition A. The mechanism cannot be made to work in condition B without changing what it does in condition A.

This is the defining characteristic of a structural failure. The mechanism is not wrong. The environment is incompatible.

**Common failure pattern:** A suppression mechanism is applied to reduce activity in the identified failure environment. Evaluation shows reduced losses in the target environment but reduced gains in adjacent environments. The suppression is adjusted — made more targeted — and the pattern repeats. The mechanism that produces losses in the failure environment is the same mechanism that produces gains in adjacent environments; targeting one affects the other proportionally.

**Dependency on conditions not detectable by the system**

When a failure pattern is conditionally predictable from external information the system does not have access to — information that could label the failure conditions in advance — but is not predictable from information the system does have access to, the failure cannot be addressed from inside the system. The discriminating signal is external to the system's observable space.

**Expected outcome disappears under realistic costs**

When the positive expected outcome of a research direction is present under idealized conditions and absent or negative under realistic costs, the direction is cost-sensitive in a way that makes the idealized result not useful. The realistic conditions are the relevant conditions.

---

## Section 3 — False Progress Signals

Signals that produce the appearance of progress without movement toward a solution.

**Metric improvement without outcome change**

A metric that improves without changing the outcome being optimized is not measuring the right thing, or is being optimized in isolation from the outcome. Training metrics improving while target-condition performance remains flat is an instance of this. The investigation focuses on the improving metric, which is the wrong focus.

**Common failure pattern:** A regularization change improves a training metric. The change is logged as progress. The target-condition performance — the original objective — has not moved. Subsequent work addresses the training metric rather than the target condition.

**Complexity increase without explanatory power**

Adding complexity — more features, more components, more parameters, more conditional logic — without increasing the system's ability to explain or predict the target outcome increases overfitting surface, computational cost, and diagnostic difficulty without reducing the failure.

**Optimization without new information**

When an optimization process converges but produces the same failure — a different configuration with the same outcome — the optimization was not the right operation. Optimizing a structurally wrong configuration produces a different wrong configuration.

The prerequisite question before optimization: does the current architecture have the capacity to solve this problem at any configuration? If no, optimization finds nothing.

---

## Section 4 — Decision Rules

Hard rules derived from the failure patterns in this research program. Actions are binary: continue or stop. "Reduce investment and continue" is not an option — that path produces the same conclusion at higher total cost.

---

**Rule 1 — Structural Failure Threshold**

*If three or more independent approaches, differing in fundamental mechanism, fail on the same target problem with identical failure signatures → classify the failure as structural.*

*Action: Stop iterating on the current layer. Evaluate whether the problem is addressable from a different architectural layer or through an orthogonal mechanism. Apply the Escalation Rule before designing any further intervention.*

---

**Rule 2 — Mathematical Infeasibility**

*If the analytically derived required performance exceeds the empirically derived achievable performance ceiling from the available signal space → the problem is not solvable within the current environment.*

*Action: Stop. Changing the model does not change the constraint. The only productive actions are: change the environment (resolution, cost structure, instrument), or close the hypothesis.*

---

**Rule 3 — Cost Sensitivity Breach**

*If the expected outcome of a research direction is positive under assumed costs and negative or zero under measured costs, and the cost difference reflects the target environment rather than measurement error → the direction is not viable.*

*Action: Stop. Evaluate whether the cost structure is changeable (instrument choice, timing, execution method). If not → close the hypothesis.*

---

**Rule 4 — Behavior Inversion**

*If a system produces the mechanism's intended outcome in baseline conditions and the opposite outcome in a specific identifiable environment, and the mechanism is the same in both cases → the failure in the specific environment is structural.*

*Action: Accept the residual. Address it through an orthogonal mechanism that operates independently of the primary mechanism — one that does not interact with it in baseline conditions.*

---

**Rule 5 — Integration Validation Prerequisite**

*If a component has not been tested end-to-end with its downstream consumer under conditions that exercise the interface → it is not validated.*

*Action: Do not advance a component to production readiness without an integration test that asserts behavioral output — not just operational output — against a known expected result.*

---

**Rule 6 — Metric Decoupling**

*If a measured metric is improving while the outcome it is intended to predict is not changing → the metric is not measuring the right thing, or is being measured in the wrong context.*

*Action: Stop optimizing the metric. Identify the decoupling cause before continuing.*

---

## Using the Rules

The rules force the structural question earlier, before the cost of continued iteration has compounded. The structural question is: is this a problem that continued work on this approach can solve, or is the constraint elsewhere?

That question can usually be answered faster than the iteration that would eventually produce the same answer.
