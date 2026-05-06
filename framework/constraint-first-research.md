# Constraint-First Research

The standard research sequence is: define a problem, build a model, evaluate the model, iterate. This sequence is not wrong. It is ordered in a way that causes certain failures to be discovered late — specifically, the failures that are detectable before any model is built.

This document describes what changes when constraints are addressed before models, and why the reordering matters.

---

## Why Most Research Is Backward

The model-first sequence places construction before validation. The first expensive operation — building and training a model — happens before the questions that would constrain the model have been answered.

This is not a mistake in any individual instance. It reflects how research proceeds by default. Models are concrete. Constraint analysis is abstract. Iteration produces feedback, which produces momentum — even when the momentum is in the wrong direction. The investment in the model also makes the structural question harder to ask honestly once the model exists. The instinct after building and training a model is to find what can be changed in the model, not to ask whether building it was the right operation.

In the mathematical limitation case, the analysis that would have closed the research direction took minutes. The iteration that eventually produced the same conclusion took months. The difference was entirely in sequencing: when the analytical question was asked relative to when the modeling work began.

**Common failure pattern:** A research direction fails to produce improvement despite multiple modeling iterations. The feasibility calculation — required performance vs achievable performance ceiling — is performed post-hoc to explain the failure. The calculation was available, unchanged, before the first model was trained.

**What most approaches do instead:**
- Begin with a model and use evaluation results to decide whether to continue.
- Treat all failures as modeling failures until architectural or environmental explanations have been exhausted.
- Compute costs and constraints as inputs to evaluation rather than as prerequisites to it.

Constraint-first research front-loads the questions that are cheap to ask and potentially decisive. It asks them before any expensive operations begin.

---

## What Constraints Are

A constraint is a fixed property of the environment that bounds what any system operating within that environment can achieve. Constraints are not model properties — they do not change when the model changes. They are not failures of the current approach — they are properties of the problem space.

Three categories of constraint are relevant to systematic research:

**Resource constraints** are limits on the computational, memory, or time budget available for an operation. They determine what representations and architectures are implementable given the hardware environment. A representation that is theoretically correct but computationally infeasible given the resource budget is not a better representation — it is inaccessible.

**Cost constraints** are the fixed costs imposed by the environment on each action the system takes. They establish the minimum performance the system must achieve to produce positive expected outcomes. A system that achieves high accuracy but does not clear the cost constraint produces negative expected outcomes regardless of its accuracy.

**Signal constraints** are limits on the information available in the input space. They bound the maximum accuracy any model can achieve — regardless of architecture or training — because accuracy is bounded by the predictability of the target given the available inputs. A model that reaches this ceiling cannot be improved by further model development.

**Common failure pattern:** Constraints are treated as model properties. When a model does not clear the cost constraint, the response is to improve the model. When a model reaches the signal ceiling, the response is to try a more complex architecture. In both cases, the constraint is environmental and does not move in response to model changes.

Identifying constraints before building guarantees that subsequent work is work the constraints do not prohibit.

---

## The Correct Sequence

**Step 1 — Feasibility**

Before building anything, determine whether the problem is solvable within the environment. Compute required performance (from cost constraints), estimate the achievable ceiling (from signal constraints), and confirm that resources are sufficient for the intended architecture (from resource constraints).

If any analysis produces a stopping condition — required performance exceeds achievable ceiling, or resources are insufficient — the subsequent steps are not worth performing. The stopping condition is the research result. Documenting it precisely is more valuable than iterating past it.

**Step 2 — Constraints**

Once feasibility is confirmed, characterize the constraints that bound the solution space. Measure actual costs from the target environment. Identify what the system cannot observe. Determine what failure modes are structural given the architecture under consideration.

This step produces a specification of what the system can and cannot do — grounded in the environment, not in a theoretical model of it. This specification constrains model design.

**Step 3 — Signal**

Given confirmed feasibility and characterized constraints, evaluate whether the available data contains a detectable signal. This is distinct from training a model — it asks whether the relationship between inputs and outputs exists, not whether a specific model can learn it.

Signal evaluation should use the simplest possible analysis: correlation, conditional distributions, mutual information. A signal not detectable by simple analysis is unlikely to be found by complex modeling.

**Step 4 — Model**

After Steps 1–3 are complete — feasibility confirmed, constraints characterized, signal verified — model development begins. At this stage, model development is constrained: the model must operate within the resource budget, under the real cost structure, on the verified signal. Approaches ruled out by the constraints are not attempted.

---

## What Reversed Thinking Looks Like

Three examples of the constraint-first sequence being applied in reverse, each from a failure in this research program:

**Model correct, environment invalid.** A classification model was built, trained, and validated. The interface between the model and the execution layer was not validated before evaluation. The model was correct. The execution layer was reading the wrong output. Constraint-first sequencing requires integration validation before evaluation — treating the integration as a constraint to verify, not an assumption to inherit.

**Signal real, cost invalid.** A research direction carried a detectable signal that survived validation. Transaction costs were estimated from published averages. Measured costs in the actual execution environment were multiples of the assumed costs, changing the viability conclusion. Constraint-first sequencing requires cost measurement before evaluation — the cost structure constrains which conclusions evaluation can produce.

**Architecture correct, resource invalid.** A visual encoding approach was architecturally sound. The computational cost of generating the encoding at training scale consumed the majority of the training budget. The architecture was correct in a context where the resource budget was not the binding constraint. It was. Constraint-first sequencing requires resource profiling at target scale before pipeline construction.

In each case, the constraint was knowable before the expensive operation. It was discovered after it.

---

## The Operational Loop

Constraint-first research changes what is iterated on and at what cost. The loop:

```
1. CONSTRAINT IDENTIFICATION
   Compute required vs achievable performance.
   Measure costs from the target environment directly.
   Profile resource usage at target scale.
   Identify what the system cannot observe.

2. HYPOTHESIS FORMATION
   State specifically what the system will do, given the constraints.
   State the conditions under which it would fail.
   State what evidence would confirm or disconfirm it.

3. MINIMUM VIABLE TEST
   Test the hypothesis with the least expensive operation that could falsify it.
   Do not build the full system to test a hypothesis a simpler test could close.

4. KILL OR PROCEED
   Stopping condition met → stop. Document the constraint precisely.
   Hypothesis confirmed → proceed to the next layer.
   Result ambiguous → identify what additional test would resolve it.
   Do not proceed past an ambiguous result.
```

The loop is not a waterfall. Constraint identification may be revisited when a test reveals a constraint missed in the initial analysis. The invariant is: constraints are checked before construction at every layer, not just at the start.

---

## The Practical Implication

The most expensive mistake in systematic research is not building the wrong model. It is building a model to solve a problem that is not solvable under the conditions it will operate in. The wrong model can be replaced. The cost of discovering infeasibility after the model has been built, the pipeline developed, the evaluation run, and the iteration performed is substantially higher than the cost of discovering it at Step 1.

Constraint-first research does not make discovery cheaper by making the problem easier. It makes discovery cheaper by making it earlier.
