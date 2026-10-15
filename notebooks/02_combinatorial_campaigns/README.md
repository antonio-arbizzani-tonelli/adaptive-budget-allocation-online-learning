# Multiple campaigns with a shared budget

[Baseline: Combinatorial-UCB](baseline_combinatorial_ucb.ipynb) · [Revision: dynamic budget pacing](dynamic_budget_pacing.ipynb) · [Project overview](../../README.md)

## Problem and setup

Four campaigns share a budget of 700 over 3,000 rounds. Their valuations are (0.95, 0.85, 0.78, 0.90), and bids lie on an 11-point grid from 0 to 1. Adjacent campaigns in the conflict chain 0–1–2–3 cannot both receive positive bids. Removing bids above each valuation leaves 33 positive campaign/bid arms and 250 feasible super-actions, including the null action.

Maximum competing bids have Beta marginals with parameters (2, 5), (2.8, 3.8), (3.5, 3), and (2.3, 4). In 30% of rounds, all campaigns share the same quantile; this creates dependence while preserving the marginals. Semi-bandit feedback reveals the reward and cost of selected arms.

Main results and ablations use 20 paired trials, with market seeds 100–119 and action seeds 1100–1119. Seed 17 is used for the illustrative environment plot.

The stochastic clairvoyant benchmark solves a true-mean LP over fixed super-action distributions, with expected cost at most $\rho=700/3000$ per round. Its expected utility is about 0.548 per round. The notebooks also simulate a sampled clairvoyant policy with its own hard-budget guard; that policy is distinct from the LP benchmark.

## Baseline

Budgeted Combinatorial-UCB estimates each arm's reward and cost, constructs a reward UCB and cost LCB, then solves an LP under a fixed spending cap $\rho$. A conservative budget guard stops bidding when the remaining budget cannot cover the maximum payment of a round.

## Observed limitation

Pseudo-regret stays near zero for part of the horizon, but the cost LCB understates expenditure. The learner spends too early, stops bidding near round 1,900, and accumulates most of its final regret afterward.

## Change

The revision estimates the winning probability of each campaign/bid pair, uses KL bounds for reward, and puts the empirical cost $b\hat p$ in the LP. The spending cap becomes

$$
\rho_t=\frac{\text{remaining budget}}{\text{remaining rounds}}.
$$

The LP excludes any super-action whose sum of bids exceeds the remaining budget. The null action provides a feasible fallback. These rules enforce the hard budget while allowing cheaper actions near the end.

## Results

Pseudo-regret sums the difference between the LP's optimal per-round expected utility and the true expected utility of each selected super-action. Realized utility and spend are reported separately.

| Policy | Mean final pseudo-regret | Mean realized utility | Mean spend / 700 |
| --- | ---: | ---: | ---: |
| Baseline Combinatorial-UCB | 595.15 | 1047.22 | 698.58 |
| Revised learner | 70.05 | 1570.45 | 699.84 |

The revised learner obtains 95.6% of the LP's expected cumulative utility. The random reference has pseudo-regret about 0.435 per round, or 1,305.6 cumulatively. All policies respect the realized budget.

The paired ablations change one component of the revised learner at a time:

| Variant | Mean final pseudo-regret |
| --- | ---: |
| KL bounds, empirical cost, dynamic pacing | 70.1 |
| Hoeffding in place of KL | 100.5 |
| Cost LCB in place of empirical cost | 166.2 |

With the cost LCB, mean spend by one quarter of the horizon is 241.4, versus 176.0 with empirical cost. These ablations support the confidence-bound and cost-estimation changes on this instance; the main baseline-to-revision comparison changes several components together.

These values use the outputs saved at [commit 38a050b](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). The revised learner's unrounded mean pseudo-regret is 70.054544.

The final presentation is available as [PDF](../../presentations/Adaptive-Budget-Allocation-for-Online-Advertising.pdf) and [PowerPoint](../../presentations/Adaptive-Budget-Allocation-for-Online-Advertising.pptx). It reports pseudo-regret and realized regret separately; these measures use expected selected-action utility and realized learner utility respectively.
