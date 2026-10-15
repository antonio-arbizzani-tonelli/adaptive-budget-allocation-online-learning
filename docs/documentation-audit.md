# Documentation evidence and checks

The numerical reference is the code and saved notebook outputs at [`38a050b32a6bd757c0c2c4db18c029caa039e919`](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). The scenario READMEs retain the simulation setups, feedback assumptions, paired seeds, ablations, metrics and hard-budget rules. No campaign was rerun for the documentation or presentation updates.

## Results and interpretation

- **Stage 2:** revised mean final pseudo-regret is 70.054544, displayed as 70.05 or 70.1. Realized regret is a separate quantity, about 73.09. Mean spend is 699.84 of 700. The saved ablations report pseudo-regret 100.5 with Hoeffding bounds and 166.2 with a cost LCB; expenditure at T/4 is 176.0 with empirical cost and 241.4 with a cost LCB.
- **Benchmarks:** Stages 1 and 2 use true-mean stochastic LPs; Stage 3 uses the best fixed distribution fitted to the realized sequence in hindsight; Stage 4 reports a segment-local true-mean LP and a stronger segment-global LP. Their reported measures are not interchangeable.
- **Stage 3 guard:** eight paired trials give identical displayed outcomes for the global and per-action guards with calibrated rates. These results support rate calibration on the tested instances, without establishing a performance gain from changing the guard.
- **Stage 4:** its primal-dual baseline already checks per-action affordability. Calibrated rates improve utility while reducing mean expenditure from 393.41 to 348.92 of 560; unused budget increases from 166.59 to 211.08. The experiments do not isolate a single cause for the remaining gap.
- **Window ablation:** SW-UCB windows 219 and 500 are compared with empirical cost and pacing fixed. The wider window improves the documented instance, whose segment duration is known. This does not establish that forgetting is irrelevant or sample count is the only limitation.
- **Scope:** claims of measured results for unavailable pacing or windowed primal-dual variants were removed. Results describe synthetic simulations, not real advertising campaigns. Finite horizon sweeps do not establish asymptotic guarantees, and CUSUM reset counts are not detection accuracy.

The earlier notebook review confirmed unchanged code cells, execution counts, saved outputs and metadata across all eight notebooks. Its only notebook edit was the final markdown discussion in `change_detection_and_pacing.ipynb`, cell 27, to remove an unsupported experiment and qualify causal claims. The saved plots retain their original labels, including some Italian text.

## Final presentation

The [PDF](../presentations/Adaptive-Budget-Allocation-for-Online-Advertising.pdf) and [PowerPoint](../presentations/Adaptive-Budget-Allocation-for-Online-Advertising.pptx) are the final files supplied by the authors and copied without content changes. They contain 23 pages and 23 slides respectively: slides 1–16 develop the main narrative, slide 17 introduces the appendix, and slides 18–23 provide technical details. The supplied PDF was not regenerated from the PowerPoint for this update.

Presentation figures and numerical claims were checked against the saved notebook evidence. This does not imply that notebook simulations were rerun or that the PowerPoint was independently rendered in every viewing application.

All 23 PDF pages were rendered and visually inspected. The PowerPoint archive is readable, the slide/page counts agree, and the numerical text extracted from each slide matches its PDF page. The published copies have the same SHA-256 hashes as the supplied files. All 60 relative documentation links resolve, and all eight notebooks and both requirements files remain unchanged from the start of this publication update.

The [presentation source map](presentation-sources.md) links the 23-slide sequence to notebook evidence. The [data snapshot](presentation-data.json) transcribes the saved configurations, metrics and numerical aggregates used for the presentation. These files also preserve the evidence references cited in the supplied slides; they contain no new simulation results.

## Verified dependency setup

[requirements.txt](../requirements.txt) pins the four scientific dependencies; [requirements-notebooks.txt](../requirements-notebooks.txt) adds optional JupyterLab and ipykernel. A clean CPython 3.12.14 environment on Windows 11 x64, with system packages excluded and pip 25.0.1, passed installation, `pip check`, import checks and the small operation checks described in the [notebook guide](../notebooks/README.md#verification-and-reproduction-limits). Scientific checks passed before and after adding notebook tools. A temporary JupyterLab server and Python kernel also passed startup and operation checks and were shut down afterward.

The full historical environment is unavailable: notebook metadata records Python 3.12.11 in one notebook and 3.10.12 in two others without a complete package manifest. The requirements describe a separately verified current environment. Transitive dependencies are not fully locked; other Python versions and operating systems have not been tested. These checks do not validate every algorithm path or guarantee identical numerical results on a new execution.

The repository description and GitHub topics are unchanged.
