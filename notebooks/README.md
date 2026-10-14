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

## Environment and kernel

Follow the [setup instructions](../README.md#run-and-explore) from the repository root. The current environment was verified with **CPython 3.12.14 on Windows 11 x64**. [requirements.txt](../requirements.txt) contains NumPy, SciPy, pandas and Matplotlib with exact direct-dependency versions. [requirements-notebooks.txt](../requirements-notebooks.txt) also pins JupyterLab and ipykernel. Standard-library modules such as `itertools` and `collections` need no installation, and SciPy provides the LP solver.

Install and launch from the repository root using the virtual environment's Python executable, as shown in the setup instructions. No shell activation is required. Installing packages with a different Python executable does not make them available to the notebook kernel.

Choose **Python 3 (ipykernel)** from that environment. If a notebook asks for an old kernel such as `.venv_ola_run`, select the new environment's kernel instead. You can check its interpreter with `import sys; print(sys.executable)` in a temporary notebook. The old display name is not a required environment or dependency. Read the baseline before its revision, and execute cells in order if you choose to rerun it.

## Verification and reproduction limits

The dependency update checked the following without executing repository notebook cells or running campaigns:

- A clean virtual environment without inherited system packages, installation from both requirements files, and `pip check` before and after adding notebook tools.
- All actual imports across eight notebooks and syntax parsing of their 92 code cells.
- Small fixed-input checks of NumPy random/array operations, SciPy Beta CDF/PPF and `linprog` with HiGHS, pandas tables and HTML output, and Matplotlib PNG rendering with the Agg backend.
- JupyterLab startup on localhost, its HTTP endpoints, and a Python kernel executing imports and a one-variable LP. The temporary server and kernel were shut down afterward.

The complete original environment is unavailable. Historical metadata records Python 3.12.11 in one notebook and 3.10.12 in two others, without a complete package manifest. The new requirements describe a compatible environment checked together, not the environment that produced the saved results. Transitive dependencies are not fully locked, and other platforms or Python versions have not been tested. These checks do not validate every algorithm path or establish numerical reproduction. Saved outputs remain the reference for published results, even if a fresh installation succeeds.

Return to the [project README](../README.md) for the main results. The existing [PDF](../Adaptive-Budget-Allocation-for-Online-Advertising.pdf) and [PowerPoint](../Adaptive-Budget-Allocation-for-Online-Advertising.pptx) are legacy versions under revision. Use the [presentation draft](../PRESENTATION_DRAFT.md) and saved notebook outputs for the corrected narrative and values.
