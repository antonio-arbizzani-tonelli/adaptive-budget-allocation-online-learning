# Notebook guide

The four scenarios study bidding with a hard budget, starting with one campaign and adding combinatorial decisions, full feedback, and changing competitor distributions.

Read each scenario's README for its setup, benchmark, metric definitions, and saved results. Open the baseline notebook first, then the revised version. Each notebook contains its own code and generated auction data.

| Scenario | Baseline notebook | Revised notebook |
| --- | --- | --- |
| [1. Single campaign](01_single_campaign/README.md) | [UCB policies](01_single_campaign/baseline_ucb.ipynb) | [Budgeted KL-UCB](01_single_campaign/budget_aware_kl_ucb.ipynb) |
| [2. Multiple campaigns](02_combinatorial_campaigns/README.md) | [Combinatorial-UCB](02_combinatorial_campaigns/baseline_combinatorial_ucb.ipynb) | [KL bounds and dynamic pacing](02_combinatorial_campaigns/dynamic_budget_pacing.ipynb) |
| [3. Primal-dual control](03_primal_dual_control/README.md) | [Reference learning rates](03_primal_dual_control/baseline_primal_dual.ipynb) | [Calibrated rates](03_primal_dual_control/calibrated_primal_dual.ipynb) |
| [4. Changing markets](04_non_stationary_market/README.md) | [SW-UCB, CUSUM-UCB and primal-dual](04_non_stationary_market/baseline_adaptive_learners.ipynb) | [Change detection and pacing](04_non_stationary_market/change_detection_and_pacing.ipynb) |

Baseline and revised comparisons share market realizations and corresponding seeds. Trial counts and auxiliary ablations vary by scenario. All results refer to the synthetic instances defined in the notebooks; the saved outputs can be read without rerunning the simulations.

Return to the [project README](../README.md) for the main results and installation instructions. The existing [PDF](../Adaptive-Budget-Allocation-for-Online-Advertising.pdf) and [PowerPoint](../Adaptive-Budget-Allocation-for-Online-Advertising.pptx) are legacy versions under revision. Use the [presentation draft](../PRESENTATION_DRAFT.md) and saved notebook outputs for the corrected narrative and values.
