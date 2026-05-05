# Case Study: Image-Based ML Compute Failure

**Status:** Post-mortem complete  
**Domain:** ML architecture / infrastructure  
**Outcome:** Approach abandoned

---

## Summary

An experiment converted OHLCV time series into candlestick chart images and trained a CNN classifier to predict directional moves. The research hypothesis was sound. The pipeline failed at scale due to GPU memory constraints, data generation bottlenecks, and training instability — before the model could be evaluated on the actual research question.

---

## Timeline

- **Phase 1:** Proof of concept — small dataset, single GPU. Image generation pipeline worked
- **Phase 2:** Scaled to full dataset (~500k images). Image generation took 18+ hours
- **Phase 3:** CNN training: OOM errors on batch sizes needed for stable training
- **Phase 4:** Reduced batch size → training instability, loss divergence
- **Phase 5:** Compute cost and time-to-insight ratio deemed unacceptable. Experiment stopped

---

## Root Cause

**Architectural mismatch between research question and compute requirements.**

The image encoding added massive data volume without adding proportional information over the raw OHLCV representation. The CNN required significant compute to learn structure that a tabular model could extract directly from the source data.

---

## Lessons

1. Prototype with 1% of data and measure compute-to-insight ratio before scaling
2. Novel data representations require justification: what information does the encoding preserve that a simpler representation loses?
3. OOM failures at scale are predictable — profile memory at small scale first
4. Research time is a resource. Pivot cost matters.

---

## What Changed

Moved to tabular representations. Image-based approaches remain in the backlog but require dedicated compute infrastructure to revisit.

---

*Data in this case study is synthetic. No live system details are included.*
