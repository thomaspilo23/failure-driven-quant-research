# More Features, Less Signal

**Type:** Model Failure

---

The feature engineering phase was thorough. Standard indicators were computed — measures of momentum, mean-reversion tendency, volatility state, trend strength, volume behavior. Each was well-understood individually. Each appeared in the research literature as useful in some context. Together they formed a feature set of substantial size.

The reasoning behind assembling them was straightforward: more information about the current state of the system should produce better predictions of future states. Additional features represented additional viewpoints on the same underlying process. More viewpoints should reduce uncertainty.

The model trained on this feature set did not perform better than simpler models. It performed comparably, and in several configurations, worse.

---

## The Assumption

The assumption was that different indicators measured different things. Features with different names, computed using different formulas, represented independent perspectives on the data. Assembling them into a single feature set was equivalent to providing the model with a richer, more complete picture of the environment.

This assumption was not tested before the features were added. The feature set grew incrementally, each addition justified on the basis of what the indicator was designed to measure. The question of whether the indicator's information was already present in the existing features was not part of the selection process.

---

## What Failed

Model performance did not improve with feature count. Validation accuracy across out-of-sample windows showed no consistent gains as features were added. Training stability declined in some configurations. The model required more compute and more regularization to avoid overfitting, without producing better results. The additional features were adding cost without adding value.

The intuition behind the feature set remained defensible. These were real quantities describing real properties of the data. But something about their combination was not working as expected.

---

## Investigation

The investigation began with a direct measurement of the information each feature contributed. Pairwise correlation analysis across the full feature set produced a result that reframed the problem.

The majority of the features were highly correlated with each other — not perfectly, but strongly enough to indicate that they were encoding substantially the same information. Momentum and trend measures computed from similar inputs over similar windows moved together. Volatility measures derived from the same underlying series captured the same variance with different labels. The feature set had been assembled by category — momentum features, volatility features, trend features — but the categories were not orthogonal. They were overlapping encodings of the same underlying signal.

Presenting a model with a large set of correlated features is not equivalent to presenting it with a large set of independent signals. In the limiting case, where all features are perfectly correlated, the entire set carries no more information than a single feature. In practice, high correlation means high redundancy: the model is shown the same information many times in slightly different encodings, with no reliable way to identify which encoding is most informative.

The consequences are predictable. In the best case, the model learns to weight one feature and largely ignore the others, effectively ignoring the cost of computing and processing the redundant ones. In worse cases, the model finds combinations of correlated features that fit the training data without capturing any generalizable structure — overfitting to the specific correlation structure of the training period, which does not persist out of sample.

---

## The Shift

The shift was from treating feature addition as enrichment to treating it as a hypothesis. Adding a feature is a claim: this feature contributes information that is not already present in the existing features. That claim should be tested before the feature is added, not assumed.

The relevant test is not whether the feature is meaningful in isolation — most standard indicators are meaningful in isolation. It is whether the feature contributes information that is independent of what the existing features already capture. This is a harder question to answer, because features that seem conceptually different often measure highly similar things when applied to the same underlying data source.

Diversity of label does not imply diversity of information.

---

## Takeaway

Feature count is not a measure of feature information. A small set of genuinely independent features will outperform a large set of correlated ones, and will generalize more reliably, because the model is learning from signal rather than from the redundancy structure of its inputs.

The discipline of measuring information content before adding features — rather than assuming that conceptual distinctiveness implies informational independence — is one of the less obvious practices in applied feature engineering and one of the more consequential ones. The tools for this measurement are not exotic: correlation analysis, mutual information, and sequential feature selection all provide direct evidence about whether a proposed feature adds independent information.

The cost of skipping this measurement is not just wasted compute. It is a model that has been trained on a representation that looks richer than it is — and that will be evaluated on out-of-sample data where the spurious structure it learned does not hold.

---

*Data and system details in this case study are synthetic and abstracted. No proprietary implementation is described.*
