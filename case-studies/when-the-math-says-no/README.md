# When the Math Says No

**Type:** Mathematical Limitation

---

The research direction seemed well-motivated. At finer time resolutions, markets contain more granular information — patterns that become invisible once averaged into longer intervals. Capturing a signal early, before it dissolved into noise, seemed like a meaningful advantage. The team built the infrastructure to test it. Signal designs were developed, evaluation frameworks were adapted, and preliminary results were examined.

Nothing worked cleanly. The natural interpretation was that the models were not yet sophisticated enough. The features needed refinement. The architecture needed adjustment. More data would help. The failure was framed as a modeling problem and iteration continued.

It continued until someone asked a different question.

---

## The Assumption

The assumption was that this was an engineering problem — that poor performance reflected limitations in the current approach that could be improved by better modeling. The transaction cost was known, it was included in every evaluation, and the expectation was that a sufficiently accurate model would clear it.

The feasibility of the problem at this resolution and cost structure was never directly examined. The question "can this work in principle?" was bypassed in favor of the more tractable question "how do we make this work?"

---

## What Failed

A mathematical audit was performed. For this time resolution, the average price move per bar fell within a measurable range. The transaction cost for a completed round-trip — entry and exit — was fixed and known. For a strategy to break even, the model's directional accuracy needed to exceed a specific threshold: the cost had to be covered by the average gain per correct prediction.

Solving for the required accuracy produced a number above 78%. Measuring the accuracy the models were actually achieving — across all configurations, signal designs, and feature sets tested — produced a range between 50% and 57%.

The gap between required and achievable was not narrow. It was not the kind of gap that better features or a better architecture would close. It was a function of the ratio between the magnitude of price moves at this resolution and the fixed cost of acting on them. That ratio was determined by market structure, not by engineering.

---

## Investigation

The investigation that followed was not technical — it was arithmetical. The cost structure implied a minimum required accuracy. The signal space implied a maximum achievable accuracy. These two numbers did not overlap, and they could not be made to overlap without changing something outside the model: the time resolution, the cost structure, or the class of instruments being traded.

What made this finding significant was its unconditional quality. The infeasibility was not conditional on the specific model used, or the specific features selected, or the specific hyperparameters tuned. It held for any model operating on any price-derived feature set at this resolution and cost structure. The problem was not unsolved. It was unsolvable under these conditions.

---

## The Shift

The shift was from "how do we solve this?" to "is this solvable?" The distinction seems obvious in retrospect. In practice, it is easy to treat every failure as an engineering failure — as a signal that the current approach needs adjustment rather than that the approach itself is wrong.

The question of solvability, once taken seriously, turned out to be answerable with simple arithmetic. That arithmetic had been available from the beginning. The information required to identify the infeasibility was present before the first model was trained. The cost of reaching this conclusion through iteration was several months of work. The cost of reaching it through prior analysis would have been a short calculation.

---

## Takeaway

Not all problems in quantitative research are modeling problems. Some are mathematical problems with definite answers. Before asking how to build a model that works in a given environment, it is worth asking whether the environment permits a working model in principle.

For any strategy that operates on thin margins — where the expected value of a correct prediction is close to the cost of an incorrect one — feasibility analysis should precede model development. The calculation is simple: what directional accuracy does this environment require, given its cost structure? What accuracy is plausibly achievable from the available signal space? If those numbers do not overlap, the environment is the problem, not the model.

The correct response in that case is not iteration. It is substitution: a different resolution, a different cost structure, a different instrument class, or an exogenous signal source that shifts the achievable accuracy range.

---

*Data and system details in this case study are synthetic and abstracted. No proprietary implementation is described.*
