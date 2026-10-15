# Presentation sources

The [PowerPoint](../presentations/Adaptive-Budget-Allocation-for-Online-Advertising.pptx) and [PDF](../presentations/Adaptive-Budget-Allocation-for-Online-Advertising.pdf) contain 16 main slides, an appendix divider and six technical backup slides. This index maps all 23 slides to the notebook code and saved outputs at source commit [`38a050b32a6bd757c0c2c4db18c029caa039e919`](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). No notebook cell or experiment was run for the presentation.

[presentation-data.json](presentation-data.json) contains the documented configurations, metrics and numerical snapshot behind the presentation's tables, labels and figures. Values retain the precision available in saved outputs; slide labels sometimes round them further for readability. The snapshot is a transcription of recorded evidence, not a new experiment or a collection of the original trial arrays.

Cell indices below start at zero and count both markdown and code cells. GitHub does not provide stable links to individual rendered notebook cells, so the notebook links are pinned to the source commit; use the listed cell index to locate the source.

## Notebook key

| Key | Versioned notebook |
| --- | --- |
| B1 | [Single-campaign baseline](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/01_single_campaign/baseline_ucb.ipynb) |
| R1 | [Budgeted KL-UCB](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/01_single_campaign/budget_aware_kl_ucb.ipynb) |
| B2 | [Combinatorial-UCB baseline](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/02_combinatorial_campaigns/baseline_combinatorial_ucb.ipynb) |
| R2 | [Dynamic budget pacing](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/02_combinatorial_campaigns/dynamic_budget_pacing.ipynb) |
| B3 | [Primal-dual baseline](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/03_primal_dual_control/baseline_primal_dual.ipynb) |
| R3 | [Calibrated primal-dual](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/03_primal_dual_control/calibrated_primal_dual.ipynb) |
| B4 | [Adaptive-learner baseline](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/04_non_stationary_market/baseline_adaptive_learners.ipynb) |
| R4 | [Change detection and pacing](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/04_non_stationary_market/change_detection_and_pacing.ipynb) |

## Source catalogue

The source IDs in the slide index resolve to these exact cells. JSON paths refer to [presentation-data.json](presentation-data.json).

| Source ID | Notebook cells | Evidence and snapshot location |
| --- | --- | --- |
| G1 | R1: 2, 3, 4, 6; R2: 3, 5, 7, 10, 12 | First-price utility and cost, stochastic oracle, LP constraints, metrics and affordability. `stage1.metrics`, `stage1.benchmark`, `stage1.hard_budget`, `stage2.metrics`, `stage2.hard_budget`. |
| G2 | R1: 8; R2: 13; R3: 12, 17; R4: 15, 17 | Paired market paths and corresponding learner seeds; main trial counts. Scenario `config` or `configuration` objects. |
| S1.1 | B1: 2, 3, 8; R1: 2, 3, 4, 6, 8 | Single-campaign setup, baseline and KL learner, stochastic LP and main table. `stage1.config`, `stage1.benchmark`, `stage1.main_results.rows`. |
| S1.2 | R1: 16, 17 | Bound-only KL/Hoeffding ablation and saved output table. `stage1.confidence_bound_ablation`. |
| S2.1 | R2: 2, 3, 5, 7, 10, 12 | Four-campaign setup, conflict graph, Beta thresholds, oracle, empirical costs and dynamic pacing. `stage2.config`, `stage2.benchmark`, `stage2.hard_budget`. |
| S2.2 | B2: 12, 14; R2: 13 | Main saved tables, normalized pseudo-regret checkpoints and original baseline plot supporting the early stopping description. `stage2.main_results.rows`. |
| S2.3 | R2: 17, 18 | Saved KL/Hoeffding and empirical-cost/LCB ablations, including expenditure at T/4. `stage2.ablations`. |
| S3.1 | B3: 6, 8, 10; R3: 1, 3, 5, 7, 8, 10, 12 | Full feedback, realized-sequence oracle, Hedge/dual rates and guards. `stage3.configuration`, `stage3.benchmark`, `stage3.algorithm_changes`. |
| S3.2 | R3: 14, 17 | Main ten-trial comparison, realized reward, distribution-based pseudo-regret, spend and budget diagnostics. `stage3.main_comparison`. |
| S3.3 | R3: 18, 19 | Eight-trial paired guard ablation with identical displayed outcomes. `stage3.guard_ablation`. |
| S3.4 | R3: 21 | Nine saved horizon rows, eight trials per horizon, fixed B/T. `stage3.horizon_sweep`. |
| S4.1 | B4: 10, 12; R4: 1, 2, 4, 6, 8, 10, 12, 15 | Three-segment setup, local/global expected-policy oracles, learner changes, detector parameters and hard-budget rules. `stage4.configuration`, `stage4.benchmarks`, `stage4.algorithm_changes`. The lowest-mean competitor distribution in each segment follows analytically from the configured Beta parameters, rather than an additional simulation. |
| S4.2 | B4: 17; R4: 17, 18 | Thirty-trial baseline/revised tables, local and global gaps, saved SEM, spend and normalized revised gaps. `stage4.main_comparison`. |
| S4.3 | R4: 19, 20 | Paired window ablation, holding empirical costs and pacing fixed. `stage4.window_ablation`. |
| S4.4 | B4: 20; R4: 22 | Recorded CUSUM reset counts and diagnostic plots. `stage4.cusum_diagnostics`. Counts are not detection accuracy. |

## Slide index

Slide numbers below are physical page positions in both files. The six appendix slides are also labeled A1–A6 in the presentation.

| Slide | Content | Source IDs |
| ---: | --- | --- |
| 1 | Title and authors | Editorial title; no numerical claim |
| 2 | Auction decision, utility and campaign budget | G1 |
| 3 | Main results table | S1.1, S2.2, S3.2, S4.2 |
| 4 | Configurations and paired comparisons | G2, S1.1, S2.1, S3.1, S4.1 |
| 5 | Single-campaign problem, baseline and revision | S1.1 |
| 6 | Single-campaign regret, SEM, spend and utility share | S1.1 |
| 7 | Combinatorial problem, conflict graph and revision | S2.1, S2.2 |
| 8 | Four saved normalized pseudo-regret checkpoints | S2.2 |
| 9 | Combinatorial regret, utility and spend | S2.2 |
| 10 | Primal-dual decision and calibrated rates | S3.1 |
| 11 | Primal-dual main comparison | S3.2 |
| 12 | Segmented-market setup and expected oracles | S4.1 |
| 13 | UCB window, spending control and primal-dual changes | S4.1 |
| 14 | Local-oracle gaps and recorded SEM | S4.2 |
| 15 | Supported findings and interpretation limits | S1.2, S2.3, S3.2, S3.3, S3.4, S4.3, S4.4 |
| 16 | Repository, notebook key and evidence reference | Source commit and this document; no additional result |
| 17 | Appendix divider | Navigation to A1–A6; no additional result |
| 18 · A1 | Benchmark and metric distinctions | G1, S3.1, S4.1 |
| 19 · A2 | Single-campaign confidence-bound ablation | S1.2 |
| 20 · A3 | Combinatorial confidence/cost ablations and early spend | S2.3 |
| 21 · A4 | Primal-dual guard ablation | S3.3 |
| 22 · A5 | Primal-dual horizon sweep | S3.4 |
| 23 · A6 | Window/spend comparisons and CUSUM reset diagnostics | S4.2, S4.3, S4.4 |

## Figures, precision and metric definitions

The graphical comparisons summarize saved numerical values: slide 6 uses the Stage 1 main means and SEM; slide 11 uses the Stage 3 main normalized pseudo-regret; slide 14 uses the six Stage 4 local-gap means and saved SEM; appendix slides 19, 20 and 23 show the confidence-bound, cost and window comparisons. Their numerical inputs are available in the JSON snapshot. The figures on slides 8 and 22 are raster summary plots; their recorded points are specified below.

Slide 8 joins four recorded checkpoints at rounds 750, 1,500, 2,250 and 3,000. The baseline values are `[0.0385, -0.0033, 0.0819, 0.1984]`; the revised values are `[0.0500, 0.0349, 0.0275, 0.0234]`. Straight connecting lines guide the eye between these checkpoints. They do not reconstruct complete simulation trajectories or introduce intermediate observations. Numerical SEM summaries were not saved for this table, and the figure has no error bands. The negative partial-horizon value is retained: the comparator is an average-cost LP rather than a pathwise optimum at each round.

Slide 22 places markers at the actual nine tested horizons: `[100, 200, 300, 500, 800, 1200, 1600, 2400, 3000]`. It uses the two saved four-decimal normalized pseudo-regret series in `stage3.horizon_sweep`, on a numerical horizon axis. B/T is held fixed at 700/3,000 and each horizon uses eight trials. Connecting lines do not establish an asymptotic rate; the nonstationary series has a small increase between the first two horizons.

The benchmarks and reported measures are checked against the notebook implementations:

- **Stages 1 and 2:** a stochastic clairvoyant LP selects a fixed bidding distribution from the true expected reward and cost, subject to expected average cost at most B/T. Stage 1 reports that expected cumulative oracle utility minus realized learner utility. Stage 2 reports pseudo-regret against the true expected reward of the selected actions; realized regret is stored separately. The sampled clairvoyant reference policy is distinct from the LP objective.
- **Stage 3:** the LP fits the best fixed distribution to the complete realized reward/cost sequence in hindsight. The reported pseudo-regret sums `F_t · x_star - F_t · x_t`, using the proposed learner distribution. Realized utility and spend come from the executed action, including any budget-guard override.
- **Stage 4:** the main comparator solves a separate true-mean LP in each stationary segment, each with cap B/T. The stronger segment-global oracle may shift expected expenditure between segments under a single total expected budget. Reported gaps subtract realized learner utility from the expected oracle utility. Neither comparator is a trajectory-wise realized optimum.

The average LP caps and the hard realized budget are distinct: affordability filters or guards enforce the latter. Configurations, seeds and feedback are preserved in the snapshot and the [scenario READMEs](../notebooks/README.md).

Stage 1 retains the fixed LP cap and a cost LCB after the KL revision; dynamic pacing and empirical LP costs enter in Stage 2. Both original Stage 1 agents stop below remaining budget 1, while the revision filters individual bids. The Stage 2 baseline stops below the worst-case round payment of 1.8; the revision filters affordable super-actions and updates its spending rate. Main comparisons share market realizations and corresponding learner seeds. The Stage 1 bound ablation reuses the main KL runs, while the Stage 3 guard ablation has eight trials rather than the main ten. These distinctions are preserved in the source snapshot.

Saved Stage 1 main tables provide six-decimal regret and numerical SEM, rounded to two decimals in slide 6. Stage 2 main tables retain their saved precision in the snapshot, while its ablations are available only as one-decimal printed means. Stage 3 main and guard comparisons store normalized regret to three decimals and reward/spend to one decimal; the horizon series stores four decimals. Stage 4 main tables store means and SEM to two decimals, while its window ablation stores one-decimal means and its printed oracle totals one decimal. Full-precision trial arrays are unavailable. No additional precision or SEM is inferred where numerical summaries were not saved. Displayed SEM is a sample standard error, not a confidence interval; the one-decimal oracle totals must not be used to reconstruct exact gaps.

The [documentation audit](documentation-audit.md) explains the numerical reconciliation to the saved Stage 2 result of 70.054544, other corrected values, and the interpretation limits applied throughout.
