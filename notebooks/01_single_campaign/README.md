# Single campaign under a hard budget

[Baseline: UCB policies](baseline_ucb.ipynb) · [Revision: budgeted KL-UCB](budget_aware_kl_ucb.ipynb) · [Project overview](../../README.md)

## Problem and setup

A bidder selects one bid per round in a first-price auction. The campaign has 5,000 rounds, budget 125, and value 0.6 per win. Three competitors bid independently from Uniform[0, 1], so a bid $b$ wins with probability $b^3$. The playable grid is 0.0, 0.1, …, 0.6; bids above the valuation are excluded.

The main comparison uses 10 paired trials. Both notebooks generate the same market paths with seed 20260517, and the budgeted policies use action seeds $20000+j$ for trial $j=0,\ldots,9$.

The stochastic clairvoyant benchmark knows the true win probabilities and solves an LP over fixed bid distributions with expected cost at most $\rho=125/5000=0.025$ per round. It assigns about 3.4% to bid 0.3 and 96.6% to bid 0.4, yielding expected utility about 0.01264 per round. It knows the distribution, rather than future auction outcomes.

## Baseline

Budget-blind UCB1 chooses bids from reward estimates. Budgeted UCB uses a reward upper confidence bound and a cost lower confidence bound in an LP, with a fixed spending cap $\rho$. Both baseline policies stop bidding when the remaining budget falls below 1, a conservative guard on this bid grid.

## Observed limitation

The Hoeffding cost lower bound often stays at zero, so the LP treats bids as cheaper than they are. The two policies stop bidding before the horizon and achieve similar regret despite the additional budget constraint.

## Change

The revised policy estimates the Bernoulli probability of winning at each bid and derives reward and cost estimates from that probability:

$$
\hat r(b)=(v-b)\hat p(b),
\qquad
\hat c(b)=b\hat p(b).
$$

KL intervals replace Hoeffding intervals. The LP uses an upper bound on win probability for reward and a lower bound for cost; it retains the fixed spending cap $\rho$. An affordability filter removes bids above the remaining budget, while bid zero remains available.

## Results

Final regret is the clairvoyant LP's expected cumulative utility minus the learner's realized cumulative utility. The notebooks also report pseudo-regret using the true expected utility of selected bids.

| Policy | Mean final regret | Mean spend / 125 | Share of LP expected utility |
| --- | ---: | ---: | ---: |
| Budget-blind UCB1 | 40.18 | 124.29 | 36.4% |
| Budgeted Hoeffding UCB | 39.55 | 124.38 | 37.4% |
| Budgeted KL-UCB | 11.37 | 123.20 | 82.0% |

The paired confidence-bound ablation keeps the revised policy structure and changes only KL to Hoeffding. Mean regret rises from 11.37 to 35.72, while mean spend falls from 123.20 to 45.55. On this instance, the wider bound keeps selecting low bids that rarely win.

An additional sweep uses 10 trials at horizons 1,000, 3,000, and 5,000, with $B/T$ held fixed. Its normalized regret is not monotonic across those horizons; it should not be read as evidence of an asymptotic regret guarantee.

These results describe the saved simulations on this auction instance. The affordability rule enforces the realized budget throughout.
