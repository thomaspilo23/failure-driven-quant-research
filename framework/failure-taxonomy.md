# Failure Taxonomy

A classification of failure types observed across this research program. Each type has a definition, a system layer, a characteristic detection difficulty, and a typical misdiagnosis. The goal is not completeness — this taxonomy covers only what was observed — but precision: each category is defined tightly enough to be distinguishable from the others.

---

## The Fixable / Structural Distinction

Before the taxonomy, one distinction cuts across all categories and determines the appropriate response to any failure:

**Fixable failures** have a specific mechanism that, once identified, can be corrected without changing the architecture of the system. The problem is in a component, an assumption, or a parameter — not in the design itself. Fixing it does not require changing what the system does; it requires changing how the system does it.

**Structural failures** are properties of the architecture. The system is behaving correctly given its design, but the design produces the failure under specific conditions. Correcting a structural failure from inside the system that produced it is not possible without changing what the system does, which typically changes how it behaves in other conditions. The correct response to a structural failure is not iteration — it is architectural change or explicit acceptance of the residual as a constraint.

Misidentifying a structural failure as a fixable one is among the most expensive errors in systematic research. It produces a cycle of intervention, measurement, and re-intervention that consumes resources without converging. The four-experiment case in this repository is the clearest example.

---

## Failure Types

---

### 1. Execution Failure

**Definition:** The system performs all prescribed operations correctly but produces an outcome that is not the intended outcome. The failure is at the boundary between components, or between the system and the environment, not within any individual component.

**System layer:** Integration boundary — the interface between independently developed and tested components.

**Why it is hard to detect:** Each component, tested in isolation, behaves correctly. The standard indicators of system health — successful initialization, correct logging, completed runs, absence of errors — do not observe what happens at the interface. A system can report success on every observable metric while producing the wrong outcome at the integration point.

**Typical false diagnosis:** Model error, data error, threshold misconfiguration. The investigation focuses on components rather than the boundary between them, because components are where prior work was done and where intuition points.

**Case reference:** *The Model That Never Traded* — a classification model produced correct predictions that were systematically misread by the execution layer due to an inverted index at the interface.

**Fixable or structural:** Fixable, once identified. The mechanism is specific and localized. The fix is a component-level correction at the interface, not an architectural change.

---

### 2. Infrastructure Failure

**Definition:** A constraint imposed by the runtime environment — hardware, computational resources, or platform behavior — makes the intended approach infeasible regardless of its correctness at the algorithmic level.

**System layer:** Infrastructure — the hardware, platform, and resource management layer on which the system runs.

**Why it is hard to detect:** Infrastructure constraints are often discovered at scale, not at prototype stage. A pipeline that works correctly on a small dataset may be entirely dominated by a resource cost that becomes prohibitive at full scale. The failure manifests not as an error but as an unacceptable tradeoff: the system runs, but too slowly, with too little resource remaining for the intended operation.

**Typical false diagnosis:** Model inefficiency, data loading bottleneck. The investigation targets the most computationally visible component — usually the model — rather than the preprocessing or representation step that precedes it.

**Case references:** *The Rendering Bottleneck* — a visual encoding step consumed the majority of the training epoch, leaving the model underutilized. *The Wrong Account* — a platform function selected among available instances by undocumented internal logic, making destination non-deterministic in multi-instance environments.

**Fixable or structural:** Mixed. The rendering bottleneck was structural given available hardware: the representation choice was incompatible with the resource budget. The routing failure was fixable: explicit binding and verification replaced implicit selection.

---

### 3. Data Failure

**Definition:** An assumption about the data or environment embedded in the research methodology is false in a way that systematically biases conclusions. The model and signal may be correct; the measurement environment is not.

**System layer:** Measurement — the layer that translates real-world quantities into inputs for the research system.

**Why it is hard to detect:** Data failures do not produce errors. They produce results that look valid because the internal consistency of the system is preserved — inputs, processing, and outputs are all coherent. The failure is only visible when the system's outputs are compared against the actual environment they were designed to model.

**Typical false diagnosis:** Model overfitting, signal decay, out-of-sample degradation. The investigation focuses on the model's generalization rather than on the validity of the inputs used to train and evaluate it.

**Case reference:** *The Cost Assumption* — transaction cost estimates drawn from published averages differed from real execution costs by multiples, not margins, in a way that changed the viability conclusion for instruments carrying the majority of expected return.

**Fixable or structural:** Fixable, but requires replacing the data source rather than changing the model. The assumption must be identified specifically before it can be corrected.

---

### 4. Model Failure

**Definition:** The model fails to generalize in a way that is attributable to the model's construction — its architecture, its training process, or the representation of its inputs — rather than to the environment or execution layer.

**System layer:** Modeling — the layer that learns from data and produces outputs used by downstream systems.

**Why it is hard to detect:** Model failure in systematic research is ambiguous by nature. A model that underperforms out-of-sample may have overfit to the training data, may be operating on insufficient data, may be using uninformative features, or may be correctly learning a signal that does not generalize — each requiring a different response. The correct diagnosis requires isolating which of these is active.

**Typical false diagnosis:** Data insufficiency, hyperparameter misconfiguration, architecture mismatch. These are easier to address than the more fundamental diagnosis — that the feature representation carries redundant information and the model is learning from noise — which requires reconsidering the input construction.

**Case reference:** *More Features, Less Signal* — a feature set assembled from thirty indicators was dominated by pairwise correlations above 0.85, producing a representation in which the majority of features encoded the same signal with different labels. Model performance declined with feature addition rather than improving.

**Fixable or structural:** Fixable, but the fix is in the input construction, not the model. Structural when the redundancy reflects a fundamental property of the data source — when the signal space genuinely contains only a few independent dimensions regardless of how many derived features are computed.

---

### 5. Mathematical Limitation

**Definition:** A research direction is infeasible in principle, given the fixed constraints of the environment, regardless of model quality or engineering effort. The infeasibility is derivable analytically from the relationship between the environment's parameters — cost structure, signal magnitude, achievable accuracy ceiling — without training any model.

**System layer:** Problem formulation — prior to any implementation.

**Why it is hard to detect:** It is not detected at all without explicit analysis. When research proceeds model-first, the infeasibility is discovered through iteration: the model improves but the system does not, and the investigation focuses on what is wrong with the model rather than on whether the problem is solvable. The analysis that would have caught the infeasibility is never performed because the question is never asked.

**Typical false diagnosis:** Insufficient model capacity, poor feature engineering, data quality issues. All of these are addressed while the mathematical constraint remains invisible.

**Case reference:** *When the Math Says No* — required directional accuracy to break even exceeded 78% given the cost structure and time resolution. Maximum measured accuracy across all configurations was 57%. The gap was a property of the ratio between price move magnitude and transaction cost at this resolution, not of any model parameter.

**Fixable or structural:** Structural. The constraint is environmental. The fix requires changing the environment — time resolution, cost structure, instrument class — not improving the model. Within the stated environment, the problem has no solution.

---

## Summary

| Type | Layer | Fixable | Detection requires |
|------|-------|---------|-------------------|
| Execution | Integration boundary | Yes | End-to-end behavioral test |
| Infrastructure | Resource / platform | Mixed | Scale profiling, explicit binding |
| Data | Measurement inputs | Yes | Empirical measurement vs assumption |
| Model | Learning / representation | Yes | Feature information analysis |
| Mathematical | Problem formulation | No — change environment | Analytical feasibility check |
