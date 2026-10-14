# Documentation evidence and corrections

This revision uses the notebook code and saved outputs at source commit [`38a050b32a6bd757c0c2c4db18c029caa039e919`](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). The outputs are the reference for numerical claims. No experiment was rerun, and no algorithm, code cell, saved output, or notebook metadata was changed.

Before publication, a fresh remote check confirmed `main` as the public default branch and this commit as its HEAD. The configured remote is `https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning.git`.

The root README gives the practical problem, approach, and main results. The scenario READMEs retain setups, feedback assumptions, benchmarks, paired seeds, ablations, and hard-budget rules. [PRESENTATION_DRAFT.md](../PRESENTATION_DRAFT.md) provides a revised outline and design brief. The [presentation source map](presentation-sources.md) and numerical snapshot document the evidence for that draft.

## Corrections applied

- **Stage 2 values:** the revised learner's saved mean final pseudo-regret is 70.054544, reported as 70.05 or 70.1 depending on precision. The README and presentation draft use these values instead of approximately 72. Saved ablations report 100.5 for Hoeffding bounds and 166.2 for a cost LCB, rather than approximately 101 and 165. Early empirical-cost spend is 176.0 at one quarter of the horizon, rather than 177; the LCB variant reports 241.4. The legacy PDF and PowerPoint still contain the older figures and are marked under revision.
- **Benchmark definitions:** Stages 1 and 2 use true-mean stochastic LP benchmarks; Stage 3 uses the best fixed distribution fitted to the realized sequence in hindsight; Stage 4 uses a segment-local true-mean LP and also reports a stronger segment-global LP. The reported measures therefore differ across scenarios.
- **Stage 3 budget guard:** the eight-trial paired guard ablation gives identical displayed results with calibrated rates. The documentation attributes the observed gain to rate calibration on these instances and makes no performance claim for the guard change.
- **Stage 4 affordability:** the baseline primal-dual learner already checks the sampled super-action. Its revision changes learning rates; per-action affordability is not a newly introduced improvement in this scenario.
- **Stage 4 expenditure:** calibrated primal-dual improves reward while spending less: 348.92 of 560, compared with 393.41 for the baseline. It leaves 211.08 unused, compared with 166.59. The remaining gap is not attributed to one proven cause.
- **Window evidence:** the paired SW-UCB ablation compares windows 219 and 500 while holding empirical cost and pacing fixed. Its observed improvement supports this window choice on the documented instance; it does not show that forgetting is irrelevant or that sample count is the sole limitation.
- **Unsupported variants:** statements claiming measured results for a pacing coefficient of 1 or a windowed primal-dual variant were removed from revised documentation. No corresponding implementation or saved experiment is available in the published notebooks. Windowed primal-dual is described only as unimplemented future work in the final markdown discussion of the Stage 4 revised notebook.
- **Interpretation:** results are presented as evidence from synthetic simulations. The repository does not evaluate real advertising campaigns or establish an asymptotic guarantee from the finite horizon sweeps.

## Presentation status and remaining limits

The committed PDF and PowerPoint remain the original legacy presentations. They are under revision and are not an aligned rendering of the new draft. The source map and data snapshot support the proposed draft's claims and figures; they do not document every claim or figure in the legacy binaries.

The draft's closing slide uses the current repository URL. The old GitHub link and unsupported historical claims in the legacy binaries remain part of the pending redesign, rather than changes made to those files in this revision.

Locally generated presentation drafts, exports, previews, and build files are excluded from the publication commit. The requested handoff is the written draft and design brief for a later redesign. No new deck or PDF is published as part of this documentation revision.

The saved notebook plots contain a mixture of English and Italian labels. Their outputs remain intact. The proposed draft can use summary charts transcribed from saved aggregates, with each chart linked to its source; such summaries would not recreate the original time-series curves.

The historical experiment environment was not recorded completely. A later dependency update adds pinned direct dependencies for a separately verified current environment, described in the [notebook guide](../notebooks/README.md#environment-and-kernel). It does not reconstruct the environment that produced the saved outputs or claim identical numerical results on a new execution. Historical presentation differences cannot be explained reliably from the recorded artifacts alone.

The repository description and GitHub topics are outside this revision and are unchanged.

## Checks performed

- Compared all eight notebook structures with the pinned source commit. Code cells, execution counts, outputs and notebook metadata are unchanged. The only notebook edit is the final markdown discussion in `change_detection_and_pacing.ipynb`, cell 27, which removes an unsupported experiment and qualifies causal claims.
- Confirmed that the repository PowerPoint and PDF have the same Git object hashes as the reference commit. Local generated slide artifacts remain outside the repository and publication commit.
- Checked 60 relative documentation links, including linked notebook files, presentations and README heading anchors. Confirmed that all 22 draft slides contain essential text, a recommended visual and a source reference.
- Traced the documented results and ablations to saved tables or printed output. Checked the draft independently for both scenario pairs. This is static verification of recorded evidence, not a clean execution or new simulation.

No algorithm tests, notebook execution or presentation rendering form part of this documentation-only publication. The redesigned deck and its PDF still require production and visual verification in the later design workflow.

## Dependency setup update

The dependency update starts from `main` at [86fe533](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/86fe533c9d559a07238efc67d832141b31809556). It pins the four imported scientific packages separately from the optional JupyterLab/kernel tools. A clean CPython 3.12.14 environment on Windows 11 x64, with system packages excluded and pip 25.0.1, passed installation, `pip check`, import checks and the small operation tests listed in the [notebook guide](../notebooks/README.md#verification-and-reproduction-limits). The scientific checks passed both before and after installing notebook tools, and a temporary JupyterLab server and kernel passed startup checks.

This is a current compatible setup, not a recovered historical environment. Only dependency files and documentation change in this update. No repository notebook cell, saved output, algorithm or presentation changes, and no campaign is rerun.

The update also checks 68 relative documentation links and confirms unchanged Git object hashes for all eight notebooks and both presentation files against its starting commit. The published results section remains unchanged.
