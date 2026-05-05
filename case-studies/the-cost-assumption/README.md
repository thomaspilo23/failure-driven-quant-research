# The Cost Assumption

**Type:** Data Failure

---

The strategy had been evaluated carefully. Walk-forward validation across multiple years of data, reasonable sample sizes, cost estimates drawn from published sources and industry references. The results were consistent. The evaluation criteria passed. The strategy appeared viable and was moved toward the next stage.

When the strategy was connected to real market conditions and real execution data, the picture changed. Not catastrophically — the strategy did not fail outright. It degraded. The edge that had been measured in evaluation was present but smaller than expected. In some configurations, it had effectively disappeared.

---

## The Assumption

Transaction costs in a systematic strategy evaluation are almost always approximations. The actual cost of executing a trade in a live market depends on the specific instrument, the time of day, the prevailing liquidity conditions, and the size of the order being placed. Published cost estimates represent some average of these factors across a broad range of conditions — not the specific conditions under which this strategy, in these instruments, at these times, would actually trade.

The assumption was that published estimates were close enough to real costs that the difference would be small and non-directional — noise, not bias.

---

## What Failed

Real execution data was collected from the actual target environment: not simulated, not estimated, but measured from real fills on the specific instruments under evaluation. The comparison against backtested cost assumptions produced a result that was not uniform across instruments.

For one instrument, the actual costs were substantially higher than the backtest had assumed — not by a marginal amount, but by a multiple. This instrument had carried a significant portion of the strategy's expected return. The higher costs did not merely reduce the edge; they eliminated it. For another instrument, the actual costs were substantially lower than assumed, suggesting the backtest had been overly conservative in that case. These errors did not cancel each other out. They combined in a direction that made the portfolio look considerably more attractive on paper than it was in the execution environment.

The mechanism was precise. The strategy's edge was real but thin. A thin edge is structurally sensitive to cost assumptions, in a way that a strategy with a wide edge is not. An error of a few basis points per trade, repeated across hundreds of trades over an evaluation period, produces a large cumulative divergence. The backtest had not been wrong about the signal. It had been wrong about the environment the signal was operating in.

---

## Investigation

The source of the discrepancy was the difference between how costs were modeled and how they actually behaved. Published spread estimates reflect average market conditions across a broad sample of time and participants. The strategy traded at specific times, in specific sessions, under specific liquidity conditions. The actual cost experienced by this strategy at these times was not the average cost — it was a session-specific, instrument-specific, liquidity-specific cost that the published average did not represent.

Two instruments in the same asset class, both with published estimates that appeared similar, had costs that diverged substantially in practice because they traded in different liquidity regimes at the times the strategy was active. The backtest had treated them as interchangeable. The market had not.

---

## The Shift

The realization was that cost assumptions are not inert parameters. They determine which strategies survive the evaluation filter and which do not. A strategy with a thin edge and an overstated cost estimate will appear to fail evaluation and be discarded. A strategy with a thin edge and an understated cost estimate will appear to pass evaluation and be advanced. In both cases, the error is in the cost assumption, not in the strategy.

This means that the accuracy of cost modeling is as important as the accuracy of signal modeling, for any strategy that depends on thin margins. Treating costs as a secondary concern — something to be approximated and refined later — inverts the correct priority. For strategies where the edge is small relative to the cost, costs should be measured first, before signal evaluation begins.

---

## Takeaway

Transaction cost assumptions should be treated as empirical claims, not as parameters. For any strategy operating on thin margins, those claims should be verified against real execution data before the strategy is evaluated. Published estimates and industry averages are inputs of last resort — appropriate only when direct measurement is unavailable. When direct measurement is possible, the assumption should be replaced by the observation.

The practical implication is sequencing: measure actual execution costs from the target environment first, then evaluate strategies against those measured costs. A strategy that appears viable against estimated costs and unviable against measured costs is not a viable strategy. Discovering this after rather than before strategy development represents a correctable sequencing error, not bad luck.

The deeper implication is that evaluation results are only as valid as the assumptions embedded in them. Cost assumptions are among the most consequential of those assumptions, and among the most commonly left unverified.

---

*Data and system details in this case study are synthetic and abstracted. No proprietary implementation is described.*
