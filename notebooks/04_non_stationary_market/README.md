# Bidding in a changing market

[Baseline: adaptive learners](baseline_adaptive_learners.ipynb) · [Revision: change detection and pacing](change_detection_and_pacing.ipynb) · [Project overview](../../README.md)

## Problem and setup

Four campaigns share budget 560 over 3,000 rounds, divided into three equal segments of 1,000 rounds. Competitor thresholds are independent Beta draws within each segment; their parameters change between segments, rotating which campaign is cheapest. The number of changes, two, is known by construction.

Campaign values are (0.95, 0.88, 0.78, 0.92). Bids lie on the grid (0, 0.10, 0.20, 0.30, 0.40, 0.55, 0.70, 0.85), subject to a chain conflict graph. There are 27 positive campaign/bid arms and 168 feasible super-actions.

Sliding-Window Combinatorial-UCB and CUSUM Combinatorial-UCB receive semi-bandit feedback. The primal-dual learner receives full feedback. Main results use 30 paired trials with market seeds 1000–1029. Agent seeds add 10000 for SW-UCB, 20000 for CUSUM-UCB, and 30000 for primal-dual. Seed 23 is used for the illustrative plot.

The main benchmark is a segment-local oracle: it solves a true-mean LP in each segment with the same average cap $\rho=B/T$. Its expected cumulative utility is 1228.6. A secondary segment-global oracle can shift expected expenditure between segments under one global budget and obtains 1252.0. Both use the known distributions; neither is the realized optimum for a sampled market path.

## Baseline

Both UCB learners send reward UCBs and cost LCBs to an LP with fixed cap $\rho$. SW-UCB keeps observations from the last 219 calendar rounds. CUSUM maintains a detector for each arm's Bernoulli win indicator, with seven reference samples, threshold about 14.63, and additional exploration probability about 0.0698.

The full-feedback primal-dual method accumulates all past normalized gains and uses the reference learning rates. It already checks affordability per sampled super-action in this baseline.

## Observed limitation

The two UCB policies spend too early, limiting exploration after later changes. The primal-dual policy leaves about 166.6 of 560 unused. Sparse observations per arm also limit CUSUM detection: the baseline records about 18.2 resets per trial. The comparison value of 54 is only two changes times 27 arms, not a measured target for correct detections.

## Change

Both UCB learners replace the cost LCB with empirical cost and use remaining budget divided by remaining rounds as the LP cap. SW-UCB also widens its calendar-round window to 500, half the known segment duration. The CUSUM detector and its parameters stay fixed.

The primal-dual revision changes its learning rates using the calibration from [Stage 3](../03_primal_dual_control/README.md#change). Its existing per-action budget guard remains in place. UCB action filtering and forced exploration also check affordability, so all methods enforce the realized budget.

## Results

The reported gap subtracts a learner's realized cumulative utility from the segment-local oracle's expected utility. This differs from the distribution-based pseudo-regret in Stage 3.

| Method | Baseline gap | Revised gap ± SEM | Revised gap / round | Revised spend / 560 |
| --- | ---: | ---: | ---: | ---: |
| SW-UCB | 561.99 | 416.65 ± 2.78 | 0.1389 | 559.81 |
| CUSUM-UCB | 533.30 | 344.93 ± 4.06 | 0.1150 | 559.60 |
| Primal-dual, full feedback | 715.78 | 660.35 ± 2.77 | 0.2201 | 348.92 |

CUSUM-UCB has the lowest mean gap on this instance, and its resets rise to about 32.0 per trial. Empirical cost and pacing change together, so this comparison does not isolate their individual contributions or measure detection accuracy.

The window ablation holds empirical cost and pacing fixed and uses 30 paired trials. Increasing $W$ from 219 to 500 reduces the mean local gap from 496.1 to 416.6. This supports the wider window on the selected three-segment instance; its size uses the known segment structure.

The calibrated primal-dual method improves reward while spending less than its baseline: it leaves about 211.1 unused, versus 166.6. It accumulates its full history without explicit resets or a sliding window. All three reported gaps use the same segment-local oracle. The experiments do not isolate a single cause for its remaining gap.
