# Adaptive Budget Allocation for Online Advertising

**Online learning for first-price auctions, from a single campaign to combinatorial bidding in non-stationary markets.**

This project studies a practical question in online advertising: **how can a bidding system learn profitable actions while respecting a hard budget that must last for the entire campaign?**

At each round, the learner selects a bid before observing the highest competing bid. Winning produces value, but also consumes budget:

\[
\text{reward}_{i,t} = (v_i-b_{i,t})\,\mathbb{1}\{b_{i,t}\geq m_{i,t}\},
\qquad
\text{cost}_{i,t} = b_{i,t}\,\mathbb{1}\{b_{i,t}\geq m_{i,t}\}.
\]

The objective is therefore not simply to identify the most rewarding bid. The learner must balance exploration, auction utility, and expenditure over time, under the hard constraint

\[
\sum_{t=1}^{T} c_t \leq B.
\]

The investigation begins with one stochastic campaign and gradually introduces the difficulties that make the problem realistic: multiple campaigns, combinatorial actions, shared budgets, partial and full feedback, and changing competitor behavior. Each stage starts from a principled baseline, identifies its concrete failure mode, and introduces the smallest modification needed to address it.

## The central finding

Across the experiments, **learning rewards was rarely the main bottleneck**. Performance depended on how uncertainty interacted with budget control.

Three ideas consistently mattered:

- estimating the Bernoulli probability of winning, the common source of both reward and cost;
- using confidence bounds that remain informative when winning probabilities are small;
- adapting the spending rule to the remaining budget and horizon instead of treating the initial average budget as a static constraint.

These choices turn budget management from a final safety check into an active part of the learning strategy.

## Experimental journey

### 1. Learning a useful bid under a hard budget

The first setting considers one campaign in a stochastic first-price auction. A budget-blind UCB learner and a budget-aware UCB learner based on a linear program provide the initial comparison.

The budget-aware baseline does not gain much from its additional constraint. Hoeffding confidence intervals remain wide for most of the horizon, and the lower confidence bound on cost is frequently zero. The linear program consequently sees many bids as almost free, both learners spend too early, and their final regret is nearly identical.

The revised learner estimates the probability of winning directly. Since reward and cost are deterministic functions of a win at a fixed bid, this single Bernoulli estimate provides both quantities. A KL confidence bound replaces the Hoeffding bound, and an affordability filter excludes bids that cannot be paid with the remaining budget.

This change reduces final regret from **39.6 to 11.4**, spends **123 of the available 125**, and reaches approximately **82% of the clairvoyant reward**, compared with about 36% for the baseline learners.

[Baseline notebook](notebooks/01_single_campaign/baseline_ucb.ipynb) · [KL-UCB notebook](notebooks/01_single_campaign/budget_aware_kl_ucb.ipynb) · [Detailed discussion](notebooks/01_single_campaign/README.md)

### 2. Scaling the decision to multiple campaigns

The second setting introduces four campaigns, a shared budget, and a conflict graph. Adjacent campaigns cannot receive a positive bid simultaneously, so the learner selects a feasible **super-action** rather than a single arm. Semi-bandit feedback reveals the outcome of each selected campaign.

The combinatorial UCB baseline learns the relative value of the arms, but its cost lower bounds make the budget constraint too permissive. Performance initially tracks the clairvoyant policy, then the budget runs out and only the null action remains. The resulting regret grows rapidly near the end of the horizon.

The improved decision layer combines four changes:

- one winning-probability estimate for each campaign and bid;
- KL confidence bounds for small-probability events;
- empirical cost, rather than an optimistic lower bound, inside the linear program;
- dynamic pacing based on the remaining budget divided by the remaining rounds.

Final pseudo-regret falls from approximately **595 to 72**, while the learner obtains about **96% of the benchmark reward** and distributes expenditure across the full horizon. The ablations separate the effects: empirical cost improves regret from about 165 to 72 relative to the cost LCB, while KL confidence bounds improve it from about 101 to 72 relative to Hoeffding bounds.

[Baseline notebook](notebooks/02_combinatorial_campaigns/baseline_combinatorial_ucb.ipynb) · [Dynamically paced learner](notebooks/02_combinatorial_campaigns/dynamic_budget_pacing.ipynb) · [Detailed discussion](notebooks/02_combinatorial_campaigns/README.md)

### 3. Budget control through a primal-dual learner

The third setting asks whether one algorithm can remain effective in both stochastic and highly non-stationary environments. With full feedback, the primal learner uses Hedge over the feasible super-actions, while an online gradient method updates a dual variable that acts as the price of budget consumption.

The initial implementation is safe but conservative. Worst-case normalization makes the effective Hedge updates small, and the learner leaves a substantial fraction of the budget unused.

The revised version calibrates the Hedge and online gradient learning rates to the actual update ranges. It also checks affordability on the sampled action, replacing only an unaffordable action with the null action instead of stopping all bidding based on a worst-case cost.

Average regret per round decreases from **0.249 to 0.121** in the stochastic environment and from **0.222 to 0.134** in the non-stationary environment. Budget use rises from 569 to 676 and from 558 to 597, respectively, without violating the constraint. A horizon sweep shows decreasing normalized regret in both regimes.

[Baseline notebook](notebooks/03_primal_dual_control/baseline_primal_dual.ipynb) · [Calibrated primal-dual learner](notebooks/03_primal_dual_control/calibrated_primal_dual.ipynb) · [Detailed discussion](notebooks/03_primal_dual_control/README.md)

### 4. Adapting when the auction market changes

The final setting makes competitor behavior piecewise stationary. The horizon contains three regimes, and the most attractive campaigns change at each transition. This creates two distinct challenges: the learner must forget obsolete observations, and the budget controller must preserve enough resources to explore after a change.

Three approaches are compared:

- Sliding-Window Combinatorial UCB, which forgets observations by construction;
- CUSUM Combinatorial UCB, which detects changes and resets affected statistics;
- the full-feedback primal-dual learner, which retains a single global history.

For the UCB learners, a wider window provides enough observations per arm, empirical costs prevent premature expenditure, and dynamic pacing corrects spending deviations around change points. The CUSUM detector itself remains unchanged, isolating the effect of the improved decision layer. The primal-dual method uses the calibrated learning rates from the previous stage.

CUSUM-UCB achieves the best result on this instance, reducing its final gap to the local oracle from **533 to 345**, or **0.115 per round**. Sliding-Window UCB improves from 562 to 417, while the primal-dual method improves from 716 to 660. The comparison also exposes a structural limitation: a learner designed to compete with the best fixed distribution cannot fully track a benchmark whose optimal policy changes between segments.

[Baseline notebook](notebooks/04_non_stationary_market/baseline_adaptive_learners.ipynb) · [Adaptive learners](notebooks/04_non_stationary_market/change_detection_and_pacing.ipynb) · [Detailed discussion](notebooks/04_non_stationary_market/README.md)

## Results at a glance

| Setting | Diagnosed failure | Main intervention | Baseline | Improved |
| --- | --- | --- | ---: | ---: |
| Single campaign | Confidence intervals make the budget constraint ineffective | Bernoulli win model and KL bound | Regret 39.6 | Regret 11.4 |
| Multiple campaigns | Optimistic costs exhaust the shared budget early | Empirical cost and dynamic pacing | Regret ≈ 595 | Regret ≈ 72 |
| Primal-dual, stochastic | Conservative normalization slows adaptation | Range-aware learning rates | Regret/round 0.249 | Regret/round 0.121 |
| Primal-dual, non-stationary | Conservative updates underuse the budget | Range-aware learning rates | Regret/round 0.222 | Regret/round 0.134 |
| Piecewise-stationary market | Old observations and early spending hinder relearning | Forgetting, change detection, and pacing | CUSUM gap 533 | CUSUM gap 345 |

## Experimental methodology

The comparisons are paired: baseline and improved learners run on the same environment realizations and random seeds. This ensures that differences in the curves come from the algorithms rather than different random draws.

The improvements are also evaluated through targeted ablations. Whenever possible, one component is changed at a time, including KL versus Hoeffding confidence bounds, empirical cost versus a cost lower bound, alternative window sizes, and different budget guards. This makes the source of each gain explicit and also documents modifications that did not help.

The main benchmarks are linear-programming oracles with access to information unavailable to the online learners. Depending on the setting, the comparison uses a stochastic clairvoyant, the best fixed distribution in hindsight, or a segment-local oracle.

## Technical components

- stochastic and non-stationary multi-armed bandits;
- combinatorial semi-bandit feedback;
- UCB, KL-UCB, Sliding-Window UCB, and CUSUM-UCB;
- Hedge and online projected gradient descent;
- primal-dual online optimization;
- linear programming over randomized bidding policies;
- hard-budget enforcement and dynamic budget pacing;
- paired simulations and ablation studies.

The implementation uses Python with NumPy, SciPy, pandas, and Matplotlib. The experiments are self-contained in Jupyter notebooks and include their generated outputs.

## Running the notebooks

Create a Python environment and install the required scientific packages:

```bash
python -m venv .venv
source .venv/bin/activate        # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install numpy scipy pandas matplotlib jupyter
jupyter lab
```

Each notebook can be run independently. Start with the baseline and then run the corresponding revised learner to reproduce the paired comparison.

## Technical presentation

The [PowerPoint presentation](Adaptive-Budget-Allocation-for-Online-Advertising.pptx) develops the work as a single engineering story: each stage introduces a new market constraint, diagnoses the resulting failure mode, and motivates a controlled algorithmic intervention. Its charts are generated from the current code and use English labels throughout. A fixed-layout [PDF version](Adaptive-Budget-Allocation-for-Online-Advertising.pdf) is also available for quick viewing and sharing.

**Authors:** Antonio Arbizzani Tonelli, Leonardo Arisi, Claudia Berra, Gaia Di Paolo.
