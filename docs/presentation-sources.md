# Presentation draft sources

This source index supports the proposed 22-slide [presentation draft](../PRESENTATION_DRAFT.md), based on the saved simulation evidence at source commit [`38a050b32a6bd757c0c2c4db18c029caa039e919`](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). Its recommended tables and summary charts use recorded numerical outputs. No notebook cell or experiment was run to prepare the draft.

The existing [PowerPoint](../Adaptive-Budget-Allocation-for-Online-Advertising.pptx) and [PDF](../Adaptive-Budget-Allocation-for-Online-Advertising.pdf) are earlier artifacts under revision. They have not been updated to match the corrected documentation or this proposed slide sequence. The mappings and chart recommendations below describe the textual draft rather than those binaries.

[presentation-data.json](presentation-data.json) contains the documented configurations, metrics and numerical snapshot available for the proposed tables and charts. Values retain the precision available in saved outputs. Suggested slide labels may round them further for readability. The snapshot is a transcription of recorded evidence, not a new experiment or a collection of the original trial arrays.

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

The source IDs in the draft slide index resolve to these exact cells. JSON paths refer to [presentation-data.json](presentation-data.json).

| Source ID | Notebook cells | Evidence and snapshot location |
| --- | --- | --- |
| G1 | R1: 2, 3, 4; R2: 3, 5, 7, 10, 12 | First-price utility and cost, LP constraints, metrics and affordability. `stage1.metrics`, `stage1.hard_budget`, `stage2.metrics`, `stage2.hard_budget`. R1 cell 6 additionally records the stochastic oracle distribution. |
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
| S4.1 | B4: 12; R4: 1, 2, 4, 6, 8, 10, 12, 15 | Three-segment setup, local/global expected-policy oracles, learner changes, detector parameters and hard-budget rules. `stage4.configuration`, `stage4.benchmarks`, `stage4.algorithm_changes`. |
| S4.2 | B4: 17; R4: 17, 18 | Thirty-trial baseline/revised tables, local and global gaps, saved SEM, spend and normalized revised gaps. `stage4.main_comparison`. |
| S4.3 | R4: 19, 20 | Paired window ablation, holding empirical costs and pacing fixed. `stage4.window_ablation`. |
| S4.4 | B4: 20; R4: 22 | Recorded CUSUM reset counts and diagnostic plots. `stage4.cusum_diagnostics`. Counts are not detection accuracy. |

## Draft slide index

| Proposed slide | Content | Source IDs |
| ---: | --- | --- |
| 1 | Title and authors | Editorial title; no numerical claim |
| 2 | Auction decision, utility and campaign budget | G1 |
| 3 | Main results table | S1.1, S2.2, S3.2, S4.2 |
| 4 | Benchmark and metric distinctions | G1, S3.1, S4.1 |
| 5 | Configurations and paired comparisons | G2, S1.1, S2.1, S3.1, S4.1 |
| 6 | Single-campaign problem, baseline and revision | S1.1 |
| 7 | Single-campaign regret, SEM, spend and utility-share table | S1.1 |
| 8 | Single-campaign confidence-bound ablation chart | S1.2 |
| 9 | Combinatorial problem, early spending and revision | S2.1, S2.2 |
| 10 | Four saved normalized pseudo-regret checkpoints | S2.2 |
| 11 | Combinatorial regret, utility and spend table | S2.2 |
| 12 | Confidence/cost ablation chart and early spend | S2.3 |
| 13 | Primal-dual decision and calibrated rate formulas | S3.1 |
| 14 | Primal-dual main comparison table | S3.2 |
| 15 | Guard ablation table | S3.3 |
| 16 | Primal-dual horizon sweep chart | S3.4 |
| 17 | Segmented-market setup and expected oracles | S4.1 |
| 18 | UCB window, spending control and primal-dual changes | S4.1 |
| 19 | Local-oracle gap chart and recorded SEM | S4.2 |
| 20 | Window/spend tables and CUSUM reset diagnostics | S4.2, S4.3, S4.4 |
| 21 | Supported findings and interpretation limits | S1.2, S2.3, S3.3, S3.4, S4.3, S4.4 |
| 22 | Repository, pinned evidence and source index | Links to the source commit and this document; no additional result |

## Recommended charts, precision and metric definitions

The draft recommends summary charts for slides 8, 10, 12, 16 and 19. For slide 8, use the saved Stage 1 ablation means. For slide 12, use the one-decimal printed Stage 2 ablation means. For slide 19, use the six saved local-gap means and their saved SEM. All numerical inputs are available in the JSON snapshot. These recommendations make no claim about the chart types or editability of the existing PowerPoint.

For proposed slide 10, join only four recorded checkpoints at rounds 750, 1,500, 2,250 and 3,000. The baseline values are `[0.0385, -0.0033, 0.0819, 0.1984]`; the revised values are `[0.0500, 0.0349, 0.0275, 0.0234]`. Any connecting lines should guide the eye between these checkpoints. They must not be presented as reconstructed simulation trajectories or intermediate observations.

For proposed slide 16, use a scatter chart with markers at the actual nine tested horizons: `[100, 200, 300, 500, 800, 1200, 1600, 2400, 3000]`. Its inputs should be the two saved four-decimal normalized pseudo-regret series in `stage3.horizon_sweep`. B/T is held fixed and each horizon uses eight trials. Connecting lines would not establish an asymptotic rate; the nonstationary series has a small increase between the first two horizons.

The benchmarks and reported measures are checked against the notebook implementations:

- **Stages 1 and 2:** a stochastic clairvoyant LP selects a fixed bidding distribution from the true expected reward and cost, subject to expected average cost at most B/T. Stage 1 reports that expected cumulative oracle utility minus realized learner utility. Stage 2 reports pseudo-regret against the true expected reward of the selected actions; realized regret is stored separately. The sampled clairvoyant reference policy is distinct from the LP objective.
- **Stage 3:** the LP fits the best fixed distribution to the complete realized reward/cost sequence in hindsight. The reported pseudo-regret sums `F_t · x_star - F_t · x_t`, using the proposed learner distribution. Realized utility and spend come from the executed action, including any budget-guard override.
- **Stage 4:** the main comparator solves a separate true-mean LP in each stationary segment, each with cap B/T. The stronger segment-global oracle may shift expected expenditure between segments under a single total expected budget. Reported gaps subtract realized learner utility from the expected oracle utility. Neither comparator is a trajectory-wise realized optimum.

The average LP caps and the hard realized budget are distinct: affordability filters or guards enforce the latter. Configurations, seeds and feedback are preserved in the snapshot and the [scenario READMEs](../notebooks/README.md).

Saved Stage 1 main tables provide six-decimal regret and numerical SEM; two decimals are recommended for the draft slide labels. Stage 2 main tables retain their saved precision, while its ablations are available only as one-decimal printed means. Stage 3 main and guard comparisons store normalized regret to three decimals and reward/spend to one decimal; the horizon series stores four decimals. Stage 4 main tables store means and SEM to two decimals, while its window ablation stores one-decimal means and its printed oracle totals one decimal. Full-precision trial arrays are unavailable. The draft should preserve these precision limits and omit SEM or confidence intervals where numerical summaries were not saved.

The [documentation audit](documentation-audit.md) records the correction from the old Stage 2 value near 72 to the saved 70.054544, other reconciled values, and unsupported historical claims omitted from the revision.
