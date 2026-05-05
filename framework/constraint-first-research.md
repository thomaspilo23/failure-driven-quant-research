# Constraint-First Research

The standard sequence in systematic research is: define a problem, build a model, evaluate the model, and iterate based on the evaluation. This sequence is not wrong. It is simply ordered in a way that causes certain failures to be discovered late rather than early — specifically, the failures that are detectable before any model is built.

This document describes what changes when constraints are addressed before models, and why the reordering matters.

---

## Why Most Research is Backward

The model-first sequence places construction before validation. The first expensive operation — building and training a model — happens before the questions that would constrain the model have been answered. This is not a mistake in any individual instance; it reflects a structural feature of how research tends to proceed. Models are concrete, and concrete things are easier to start working on than abstract constraint analysis. Iteration is familiar, and iteration produces feedback, which produces momentum, even when the momentum is in the wrong direction.

The cost of this sequence appears when the constraint analysis that was skipped would have changed the decision to proceed. In the mathematical limitation case, the analysis that would have closed the research direction took minutes. The iteration that eventually produced the same conclusion took months. The difference was entirely in sequencing — in when the analytical question was asked relative to when the modeling work began.

The model-first sequence also creates a psychological dynamic that makes abandonment harder. After a model has been built, trained, and evaluated, the investment in that model makes the structural question more difficult to ask honestly. The instinct is to look for what can be changed in the model rather than whether the model-building was the right operation to perform.

Constraint-first research front-loads the questions that are cheap to ask and potentially decisive. It asks them before any expensive operations begin.

---

## What Constraints Are

A constraint is a fixed property of the environment that bounds what any system operating within that environment can achieve. Constraints are not model properties — they do not change when the model changes. They are not failures of the current approach — they are features of the problem space.

Three categories of constraint are relevant to systematic research:

**Resource constraints** are limits on the computational, memory, or time budget available for an operation. They determine what representations and architectures are implementable given the hardware environment. A representation that is theoretically optimal but computationally infeasible given the resource budget is not a better representation — it is an inaccessible one.

**Cost constraints** are the fixed costs imposed by the environment on each action the system takes. They establish the minimum performance the system must achieve to produce positive expected outcomes. A system that achieves high accuracy but does not clear the cost constraint produces negative expected outcomes regardless of its accuracy.

**Signal constraints** are limits on the information available in the input space. They establish the maximum accuracy any model can achieve, regardless of its architecture or training procedure, because accuracy is bounded by the predictability of the target given the available inputs. A well-designed model that reaches this ceiling cannot be improved by further model development.

Identifying these constraints before building does not guarantee success. But it does guarantee that the work done after identification is work that the constraints do not prohibit.

---

## The Correct Sequence

**Step 1 — Feasibility**

Before building anything, determine whether the problem is solvable within the environment. This means computing the required performance (derived from cost constraints), estimating the achievable performance ceiling (derived from signal constraints), and confirming that resources are sufficient for the intended architecture (derived from resource constraints).

If any of these analyses produce a stopping condition — if the required performance exceeds the achievable ceiling, or if the resources are insufficient — the subsequent steps are not worth performing. The stopping condition found in Step 1 is the research result. Documenting it precisely is more valuable than iterating past it.

**Step 2 — Constraints**

Once feasibility is confirmed, characterize the constraints that bound the solution space. This means measuring actual costs (not assumed costs) from the target environment, identifying the features of the environment that the system will not be able to observe, and determining what failure modes are structural given the architecture being considered.

This step produces a specification of what the system can and cannot do, grounded in the environment rather than in a theoretical model of the environment. It is this specification that guides model design — not an abstract objective function.

**Step 3 — Signal**

Given confirmed feasibility and characterized constraints, evaluate whether the available data contains a signal that the system can learn. This is a distinct operation from training a model — it asks whether the relationship between inputs and outputs exists and is detectable, not whether a specific model can learn it.

Signal evaluation should be done with the simplest possible analysis: correlation, conditional distributions, mutual information. If no signal is detectable with simple analysis, the probability that a complex model will find one is low and should not be assumed.

**Step 4 — Model**

Only after Steps 1–3 are complete — feasibility confirmed, constraints characterized, signal verified — does model development begin. At this stage, the model development is constrained: the model must operate within the resource budget, under the real cost structure, and on the verified signal. Approaches that are ruled out by the constraints are not attempted.

---

## What Reversed Thinking Looks Like

The following are examples of the constraint-first sequence being applied in reverse, each drawn from a failure in this research program:

**Model correct, environment invalid.** A classification model was built, trained, and validated. The interface between the model and the execution layer was not validated. The model was correct. The execution layer was reading the wrong output. The environment — specifically, the integration assumption — was invalid. Constraint-first sequencing would have required integration validation before model evaluation, catching the failure before any evaluation results were produced.

**Signal real, cost invalid.** A research direction carried a detectable signal. The signal survived walk-forward validation. Transaction costs were estimated from published averages. The measured costs in the actual execution environment were multiples of the assumed costs, in a direction that changed the viability conclusion. Constraint-first sequencing would have required cost measurement before evaluation, changing whether the direction was advanced.

**Architecture correct, resource invalid.** A visual encoding approach was architecturally sound and theoretically motivated. The computational cost of generating the encoding at training scale consumed the majority of the training budget, leaving the model underutilized. The architecture was correct in a context where resources were not the binding constraint. They were. Constraint-first sequencing would have required resource profiling at target scale before pipeline construction.

---

## The Operational Loop

Constraint-first research does not eliminate iteration — it changes what is iterated on and at what cost. The operational loop is:

```
1. CONSTRAINT IDENTIFICATION
   Define the fixed properties of the environment.
   Compute required vs achievable performance.
   Measure costs directly.
   Identify what the system cannot observe.

2. HYPOTHESIS FORMATION
   State specifically what the system will do, given the constraints.
   State the conditions under which it would fail.
   State what evidence would confirm or disconfirm it.

3. MINIMUM VIABLE TEST
   Test the hypothesis with the least expensive operation that could falsify it.
   Do not build the full system to test a hypothesis that a simpler test could close.

4. KILL OR PROCEED
   If the test produces a stopping condition → stop. Document the constraint precisely.
   If the test confirms the hypothesis → proceed to the next layer.
   If the test is ambiguous → identify what additional test would resolve the ambiguity.
   Do not proceed past an ambiguous result.
```

The loop is not a waterfall. Steps 1 and 2 may be revisited when a test reveals a constraint that was not identified in the initial analysis. The distinguishing feature of constraint-first research is not that it proceeds linearly — it is that it always checks constraints before committing to construction.

---

## The Practical Implication

The most expensive mistake in systematic research is not building the wrong model. It is building a model to solve a problem that was not solvable under the conditions it would operate in. The wrong model can be replaced. The cost of discovering that the problem was infeasible — after the model has been built, the pipeline developed, the evaluation run, and the iteration performed — is substantially higher than the cost of discovering it at Step 1.

Constraint-first research does not make the discovery cheaper by making the problem easier. It makes the discovery cheaper by making it earlier.
