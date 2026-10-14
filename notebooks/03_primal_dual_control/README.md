# Primal-dual budget control

[Baseline: reference learning rates](baseline_primal_dual.ipynb) · [Revision: calibrated learning rates](calibrated_primal_dual.ipynb) · [Project overview](../../README.md)

## Problem and setup

Four campaigns share budget 700 over 3,000 rounds. The valuations, 11-point bid grid, Beta marginals, conflict chain, and 250 feasible super-actions match [Stage 2](../02_combinatorial_campaigns/README.md#problem-and-setup).

Two environments are compared. The stochastic environment retains the Beta marginals and 30% common-quantile mechanism. The rapidly changing environment is generated before play: five sinusoidal cycles with campaign-specific phases and Gaussian noise of standard deviation 0.05, clipped to [0, 1].

The learner chooses before observing the round's thresholds. It then receives full feedback, allowing reward and cost to be computed for every super-action. The main experiment uses 10 trials per environment. Trial $j$ uses market seed $17+1000j$, with an additional 100000 in the changing environment; primal-dual and random agent seeds add 10000 and 20000 respectively. Baseline and revised runs share these market paths and corresponding action seeds.

The benchmark is the best fixed randomized distribution in hindsight under average cost at most $\rho=B/T$. An LP fits it to the empirical means of the realized threshold sequence. It does not choose a new optimal distribution at every round or change point.

## Baseline

Hedge selects a distribution over super-actions using the Lagrangian

$$
L_t(a)=f_t(a)-\lambda_t(c_t(a)-\rho).
$$

Projected online gradient descent updates the budget multiplier within $[0,1/\rho]$, using the cost of the proposed distribution. Hedge gains are normalized with fixed worst-case bounds. The baseline rates are $\eta_H=\sqrt{\log K/T}$ and $\eta_D=1/\sqrt T$, where $K=250$. A global guard selects the null action when the remaining budget falls below the maximum round payment, 1.8.

## Observed limitation

The gain-normalization range is about 11. Combined with conservative learning rates, this produces small updates and substantial unused budget. The learner outperforms the random reference in both environments but leaves more than 130 of 700 unused.

## Change

The revision calibrates the learning rates to the normalized Hedge gains and the bound on the dual gradient:

$$
\eta_H=\sqrt{\frac{8\log K}{T}},
\qquad
\eta_D=\frac{\lambda_{\max}}{G\sqrt T},
\qquad
G=\max(\rho,C_{\max}-\rho).
$$

It updates Hedge with $f_t-\lambda_tc_t$. Removing the term $\lambda_t\rho$, which is common to all actions, leaves the selected distribution unchanged. A per-action guard replaces a sampled action with the null action only when its maximum payment exceeds the remaining budget.

Hedge and the dual update still use the proposed distribution, while budget accounting uses the executed action. This distinction matters when interpreting the metric, even though replacement frequency is zero to the displayed precision in the main outputs.

## Results

The notebook's pseudo-regret compares the reward of the hindsight distribution with the reward of the learner's proposed distribution on each realized full-feedback vector. The table reports this quantity divided by the horizon; realized reward and expenditure are separate measures.

| Environment | Version | Pseudo-regret / round | Mean realized reward | Mean spend / 700 |
| --- | --- | ---: | ---: | ---: |
| Stochastic | Baseline | 0.249 | 895.8 | 568.6 |
| Stochastic | Calibrated | 0.121 | 1287.9 | 675.8 |
| Rapidly changing | Baseline | 0.222 | 835.4 | 557.6 |
| Rapidly changing | Calibrated | 0.134 | 1100.0 | 596.8 |

All recorded main runs satisfy the hard budget. An eight-trial paired ablation gives identical displayed results for the global and per-action guards with calibrated rates. The saved evidence therefore supports rate calibration as the source of the observed gain; it does not establish a performance gain from the new guard.

A further sweep uses eight trials at nine horizons from 100 to 3,000 while keeping $B/T$ fixed. Normalized regret decreases overall, with some nonmonotonic values. This finite sweep does not establish an asymptotic guarantee. The full fixed-policy LP comparator used here is also stronger than the scaled comparator in the adversarial guarantee discussed in the baseline notebook.
