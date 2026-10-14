# Adaptive Budget Allocation for Online Advertising

Choosing a bid in an advertising auction means balancing the chance of winning, the value of a win, and the money left for later rounds. This project studies how a bidder can learn from auction outcomes while spreading a limited budget across a campaign.

The experiments compare UCB and KL-UCB bidding policies, combinatorial decisions across campaigns, primal-dual budget control, and adaptation to changing competitors. Each scenario starts with a baseline, examines an observed limitation, and evaluates a specific revision.

## Main results

| Simulated setting | Baseline → revised | Reported measure |
| --- | ---: | --- |
| Single campaign: budgeted UCB → KL-UCB | 39.6 → 11.4 | Final regret against a stochastic clairvoyant |
| Multiple campaigns: Combinatorial-UCB → KL bounds, empirical cost and pacing | 595.2 → 70.1 | Final pseudo-regret against a stochastic clairvoyant |
| Primal-dual, stochastic environment | 0.249 → 0.121 | Pseudo-regret per round against the best fixed distribution |
| Primal-dual, rapidly changing environment | 0.222 → 0.134 | Same measure and comparator |
| Piecewise-stationary market: CUSUM-UCB | 533.3 → 344.9 | Final gap to a segment-local oracle |

These are rounded means from the saved notebook outputs on synthetic auction instances. The measures use different benchmarks and should not be compared across scenarios. They provide evidence for these simulations, with no evaluation on real advertising campaigns.

The notebook outputs saved at [commit 38a050b](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919) are the reference for these values. The [documentation audit](docs/documentation-audit.md) records corrections made during this revision.

## Four scenarios

### 1. Single campaign

The bidder learns which bid to place from sparse wins. The baseline compares budget-blind UCB1 with a budgeted UCB policy whose linear program uses reward upper bounds and cost lower bounds. Wide Hoeffding intervals make the cost constraint too permissive. The revision estimates the Bernoulli win probability, uses KL bounds, and filters unaffordable bids. It reduces regret and spends 123.2 of the available 125.

[Setup, notebooks and ablation](notebooks/01_single_campaign/README.md)

### 2. Multiple campaigns

Four campaigns share a budget and a conflict graph, so each decision is a feasible combination of bids. The Combinatorial-UCB baseline receives semi-bandit feedback but spends too early. The revision combines KL win-probability bounds, empirical LP costs, and a spending rate based on remaining budget and rounds. It uses almost all the budget through the horizon and obtains 95.6% of the clairvoyant LP's expected reward.

[Setup, notebooks and ablations](notebooks/02_combinatorial_campaigns/README.md)

### 3. Primal-dual control

With full feedback, Hedge selects a distribution over feasible bid combinations and projected online gradient descent updates the price of spending. Conservative learning rates leave budget unused in the baseline. Calibrating the rates to the update ranges improves reward and expenditure in both tested environments. A separate affordability ablation finds no measurable benefit from changing the budget guard on these instances.

[Setup, notebooks and ablations](notebooks/03_primal_dual_control/README.md)

### 4. Changing markets

Competitor distributions change across three segments. Sliding-Window UCB and CUSUM-UCB use semi-bandit feedback; primal-dual control uses full feedback. The UCB baselines spend too early, while primal-dual leaves budget unused. Empirical costs and dynamic pacing improve the UCB results, together with a wider sliding window for SW-UCB. CUSUM-UCB has the lowest recorded gap. SW-UCB improves from 562.0 to 416.6, and primal-dual from 715.8 to 660.4.

[Setup, notebooks and window ablation](notebooks/04_non_stationary_market/README.md)

## Auction model and evaluation

For campaign $i$, a bid $b_{i,t}$ wins when it reaches the highest competing bid $m_{i,t}$. A first-price win yields utility $v_i-b_{i,t}$ and costs $b_{i,t}$:

$$
r_{i,t}=(v_i-b_{i,t})\mathbf{1}\{b_{i,t}\ge m_{i,t}\},
\qquad
c_{i,t}=b_{i,t}\mathbf{1}\{b_{i,t}\ge m_{i,t}\}.
$$

The learner must respect the realized budget $\sum_t c_t\le B$. The LPs use an average spending cap $\rho=B/T$, or the remaining-budget rate in the paced variants. Separate affordability rules enforce the hard budget.

| Scenario | Rounds | Budget | Main trials | Benchmark |
| --- | ---: | ---: | ---: | --- |
| Single campaign | 5,000 | 125 | 10 | Fixed bid distribution using the true stochastic means |
| Multiple campaigns | 3,000 | 700 | 20 | Fixed super-action distribution using the true stochastic means |
| Primal-dual | 3,000 | 700 | 10 per environment | Best fixed distribution fitted to the realized sequence in hindsight |
| Changing markets | 3,000 | 560 | 30 | One true-mean LP per segment, each with the same average spending cap |

Baseline and revised runs share market realizations and corresponding random seeds within each comparison. The scenario READMEs specify seeds, feedback, metric definitions, and ablations. The changing-market notebooks also include a stronger oracle that can allocate expected budget across segments.

## Run and explore

The [notebook guide](notebooks/README.md) links all eight notebooks. Each contains its setup, generated auction data, and saved outputs; no external dataset or auction service is required.

Create and activate a Python virtual environment, then install the scientific packages and JupyterLab:

```bash
python -m venv .venv
# Activate: source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install numpy scipy pandas matplotlib jupyterlab
jupyter lab
```

Run a baseline notebook before its revised counterpart to follow the comparison. Dependency versions are not pinned in this repository, so the saved outputs remain the reference for the reported results.

## Presentation

The existing [PDF](Adaptive-Budget-Allocation-for-Online-Advertising.pdf) and [PowerPoint](Adaptive-Budget-Allocation-for-Online-Advertising.pptx) are legacy versions **under revision**. Their Stage 2 results still show approximately 72 rather than the saved notebook value of 70.1; use the notebook outputs and scenario READMEs as the numerical reference.

[PRESENTATION_DRAFT.md](PRESENTATION_DRAFT.md) contains the revised outline and design brief. The [presentation source map](docs/presentation-sources.md) and its numerical snapshot support that draft, rather than the existing legacy slides.

**Authors:** Antonio Arbizzani Tonelli, Leonardo Arisi, Claudia Berra, Gaia Di Paolo.
