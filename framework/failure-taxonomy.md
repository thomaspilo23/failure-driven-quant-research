# Failure Taxonomy

A classification of failure types observed across this research program. Each type has a strict definition, a system layer, a characteristic detection difficulty, and a typical misdiagnosis. The goal is not completeness — this taxonomy covers only what was observed — but precision: each category is defined tightly enough to be distinguishable from the others.

---

## The Fixable / Structural Distinction

One distinction cuts across all categories and determines the appropriate response to any failure:

**Fixable failures** have a specific mechanism that, once identified, can be corrected without changing the architecture of the system. The problem is in a component, an assumption, or a parameter — not in the design. Fixing it does not require changing what the system does; it requires changing how the system does it.

**Structural failures** are properties of the architecture. The system is behaving correctly given its design, but the design produces the failure under specific conditions. Correcting a structural failure from inside the system that produced it is not possible without changing what the system does, which changes how it behaves in other conditions. The correct response is architectural change or explicit acceptance of the residual as a constraint.

Misidentifying a structural failure as a fixable one produces a cycle of intervention, measurement, and re-intervention that consumes resources without converging. The four-experiment case in this repository is the clearest example: the failure was structural; four fixable-framed interventions changed nothing.

---

## Failure Types

---

### 1. Execution Failure

**Definition:** The system performs all prescribed operations correctly but produces an outcome that is not the intended outcome. The failure is at the boundary between components, or between the system and the environment — not within any individual component.

**System layer:** Integration boundary — the interface between independently developed and tested components.

**Why it is hard to detect:** Each component, tested in isolation, behaves correctly. The standard indicators of system health — successful initialization, correct logging, completed runs, absence of errors — do not observe what happens at the interface. A system reports success on every observable metric while producing the wrong outcome at the integration point.

**Typical false diagnosis:** Model error, data error, threshold misconfiguration. The investigation focuses on components because that is where prior work was done. The interface is not investigated because it produced no error signal.

**Common failure pattern:** Component A and component B are each tested independently and pass. The interface between them — the contract specifying what data format, index mapping, or behavioral expectation each side assumes — is not tested. When connected, the contract is violated silently.

**Case reference:** *The Model That Never Traded* — a classification model produced correct predictions that were systematically misread by the execution layer due to an inverted index at the interface.

**Fixable or structural:** Fixable once identified. The mechanism is specific and localized. The fix is a component-level correction at the interface.

---

### 2. Infrastructure Failure

**Definition:** A constraint imposed by the runtime environment — hardware, computational resources, or platform behavior — makes the intended approach infeasible regardless of its correctness at the algorithmic level.

**System layer:** Infrastructure — the hardware, platform, and resource management layer on which the system runs.

**Why it is hard to detect:** Infrastructure constraints are discovered at scale, not at prototype stage. A pipeline that functions on a small dataset is dominated by a resource cost that becomes prohibitive at full scale. The failure manifests not as an error but as an unacceptable tradeoff: the system runs, but resource allocation is wrong in a way that cannot be resolved without changing the approach.

**Typical false diagnosis:** Model inefficiency, data loading bottleneck. The investigation targets the most computationally visible component — usually the model — rather than the preprocessing or representation step that precedes it.

**Common failure pattern:** A representation is chosen for its theoretical properties before its generation cost at training scale is profiled. The cost is discovered during training, after the pipeline is built around that representation.

**Case references:** *The Rendering Bottleneck* — a visual encoding step consumed the majority of the training epoch, leaving the model resource-starved. *The Wrong Account* — a platform function selected among available instances by undocumented internal logic, making destination non-deterministic under multi-instance conditions.

**Fixable or structural:** Mixed. Resource bottlenecks are structural when the representation choice is incompatible with the resource budget — changing the implementation does not reduce the cost of generating the representation. Routing failures are fixable: explicit binding and verification replace implicit selection.

---

### 3. Data Failure

**Definition:** An assumption about the data or environment embedded in the research methodology is false in a way that systematically biases conclusions. The model and signal may be correct; the measurement environment is not.

**System layer:** Measurement — the layer that translates real-world quantities into inputs for the research system.

**Why it is hard to detect:** Data failures produce no errors. They produce results that look valid because the internal consistency of the system is preserved — inputs, processing, and outputs are coherent. The failure is visible only when the system's outputs are compared against the actual environment they were designed to model.

**Typical false diagnosis:** Model overfitting, signal decay, out-of-sample degradation. The investigation focuses on the model's generalization rather than on the validity of the inputs used to train and evaluate it.

**Common failure pattern:** Cost estimates from published sources or prior work are applied to a specific instrument without measuring that instrument's actual costs in the actual session at the actual order sizes the system uses. Discrepancies are discovered post-evaluation, requiring re-evaluation from the beginning.

**Case reference:** *The Cost Assumption* — transaction cost estimates differed from real execution costs by multiples, not margins, changing the viability conclusion for instruments carrying the majority of expected return.

**Fixable or structural:** Fixable, but the fix requires replacing the data source — measuring the environment directly — rather than changing the model.

---

### 4. Model Failure

**Definition:** The model fails to generalize in a way attributable to the model's construction — its architecture, training process, or input representation — rather than to the environment or execution layer.

**System layer:** Modeling — the layer that learns from data and produces outputs for downstream systems.

**Why it is hard to detect:** Model failure in systematic research is ambiguous by nature. A model underperforming out-of-sample may have overfit, may be operating on insufficient data, may be using uninformative features, or may be correctly learning a signal that does not generalize — each requiring a different response. Isolating which diagnosis is active requires deliberate analysis, not intuition.

**Typical false diagnosis:** Data insufficiency, hyperparameter misconfiguration, architecture mismatch. These are addressed while the more fundamental diagnosis — that the feature representation is redundant and the model is learning structure in the noise — is not reached.

**Common failure pattern:** A candidate feature is added because it measures something conceptually distinct from existing features. Its marginal information contribution — the reduction in uncertainty about the target beyond what existing features already provide — is not computed. It is correlated at 0.87 with an existing feature. The model trains on thirty features carrying the information of four.

**Case reference:** *More Features, Less Signal* — a feature set of thirty indicators had a median pairwise correlation above 0.85. Model performance declined with feature addition rather than improving.

**Fixable or structural:** Fixable, but the fix is in the input construction, not the model. Structural when the redundancy reflects a fundamental property of the data source — when the signal space contains only a few independent dimensions regardless of how many derived features are computed from it.

---

### 5. Mathematical Limitation

**Definition:** A research direction is infeasible in principle, given the fixed constraints of the environment, regardless of model quality or engineering effort. The infeasibility is derivable analytically from the relationship between environmental parameters — cost structure, signal magnitude, achievable accuracy ceiling — without training any model.

**System layer:** Problem formulation — prior to any implementation.

**Why it is hard to detect:** It is not detected at all without explicit analysis. When research proceeds model-first, the infeasibility is discovered through iteration: the model improves but the system does not. The investigation focuses on what is wrong with the model rather than on whether the problem is solvable. The analysis that would have caught the infeasibility is never performed because the question is never asked.

**Typical false diagnosis:** Insufficient model capacity, poor feature engineering, data quality issues. All of these are addressed while the mathematical constraint remains invisible.

**Common failure pattern:** A research direction at a fine time resolution produces models with improving training metrics that do not improve realized performance. The investigation focuses on model quality — architecture, regularization, features. The cost-to-signal ratio, which makes the direction infeasible at this resolution independent of model quality, is not computed until iteration has failed to produce improvement over many cycles.

**Case reference:** *When the Math Says No* — required directional accuracy exceeded 78% given the cost structure and time resolution. Maximum measured accuracy across all configurations was 57%. The gap was a property of the environment, not of any model parameter.

**Fixable or structural:** Structural. The constraint is environmental. Improving the model does not change the constraint. The environment must change — time resolution, cost structure, instrument class — or the hypothesis must be closed.

---

## Summary

| Type | Layer | Fixable | Detection requires |
|------|-------|---------|-------------------|
| Execution | Integration boundary | Yes | End-to-end behavioral test with known expected output |
| Infrastructure | Resource / platform | Mixed | Scale profiling before architecture commitment |
| Data | Measurement inputs | Yes | Empirical measurement in place of assumed values |
| Model | Learning / representation | Yes | Marginal information analysis before feature addition |
| Mathematical | Problem formulation | No — change environment | Analytical feasibility check before model development |
