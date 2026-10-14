# Presentation draft

## Purpose and status

This is the English content and design brief for a new presentation of **Adaptive Budget Allocation for Online Advertising**. It replaces the narrative of the existing 25-slide presentation, rather than prescribing small layout repairs. The existing [PowerPoint](Adaptive-Budget-Allocation-for-Online-Advertising.pptx) and [PDF](Adaptive-Budget-Allocation-for-Online-Advertising.pdf) remain legacy versions under revision. This document is a proposal for the redesign, not a newly rendered deck.

All numerical claims below come from code and outputs saved at reference commit [`38a050b32a6bd757c0c2c4db18c029caa039e919`](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). No experiment was rerun. Cell indices are **zero-based**, counting code and markdown cells. Notebook links at the end are pinned to that commit. The [source catalogue](docs/presentation-sources.md) gives additional cross-references, and [presentation-data.json](docs/presentation-data.json) transcribes the saved aggregates, configurations and seeds.

## What to retain and change

Retain the first-price auction model, the four progressively different settings, the baseline/revision comparisons, and the ablations that test a particular decision. Keep the authors' names and the technical explanations that connect spending to learning.

Reorganize the story around a practical question: **how can a bidder learn where spending is useful while preserving money for the rest of a campaign?** Each scenario should explain what changed in that decision problem, why the baseline struggled, what the revision changed, and what the saved experiment actually shows. Algorithm names identify those decisions but should not become the organizing principle.

Replace the old combinatorial result near 72 with **70.1 pseudo-regret**, and update its spending and ablation values. Remove results for dynamic pacing in Scenario 1 and windowed primal-dual in Scenario 4, since neither has a published implementation and saved output. Do not reuse the legacy conclusion that the guard change improved primal-dual performance. Replace the old GitHub address with the repository link on slide 22.

Use the 22-slide sequence below as a complete technical presentation. For a shorter interview discussion, slides 4, 8, 12, 15, 16 and 20 can serve as backup material without changing their content. Put detailed configurations and source references in speaker notes. Keep the simulation conditions and benchmark labels visible alongside result figures.

## Slide-by-slide draft

### 1. Adaptive Budget Allocation for Online Advertising

**Essential text**

Learning bids from auction outcomes while managing a limited campaign budget.

Antonio Arbizzani Tonelli, Leonardo Arisi, Claudia Berra, Gaia Di Paolo.

**Recommended visual:** A restrained title slide. An optional conceptual campaign timeline may show repeated bid decisions and a finite budget, without a fabricated spending curve.

**Source:** Editorial project title and author credits in the existing presentation. Model implemented in [R1], cells 2–4. No numerical result on this slide.

### 2. A profitable bid must leave room for later rounds

**Essential text**

A higher bid can increase the chance of winning, but each win reduces the money available later. The bidder learns from outcomes while deciding how much of the campaign budget to risk now.

For a first-price auction:

$$r_t=(v-b_t)\mathbf{1}\{b_t\ge m_t\},\qquad c_t=b_t\mathbf{1}\{b_t\ge m_t\},\qquad \sum_{t=1}^{T}c_t\le B.$$

Budget-aware LP policies use a spending cap, either $B/T$ or remaining budget divided by remaining rounds. Primal-dual control uses $B/T$ as its average-cost target. Separate affordability checks enforce the realized budget.

**Recommended visual:** One auction decision connected to the remaining campaign budget. Define $v$ as value, $b_t$ as bid and $m_t$ as the highest competing bid. Show only this essential formula, with generous spacing.

**Source:** [R1], cells 2–4; [R2], cells 3, 5, 7, 10 and 12. For multiple campaigns, round utility and cost sum over the selected campaign/bid arms.

### 3. Results from the saved simulations

**Essential text**

| Setting | Baseline | Revised | Measure and comparator |
| --- | ---: | ---: | --- |
| Single campaign | 39.6 | 11.4 | Final regret, stochastic clairvoyant LP |
| Multiple campaigns | 595.2 | 70.1 | Final pseudo-regret, stochastic clairvoyant LP |
| Primal-dual, stochastic | 0.249 | 0.121 | Pseudo-regret per round, best fixed distribution in hindsight |
| Primal-dual, changing sequence | 0.222 | 0.134 | Same measure and comparator |
| Segmented market, CUSUM-UCB | 533.3 | 344.9 | Final gap, segment-local oracle |

These are mean outcomes on synthetic auction instances. Each comparison uses paired market paths. Different rows use different benchmarks and scales.

**Recommended visual:** A compact comparison table, with no common bar axis or cross-scenario ranking. Include the trial counts from slide 5 in a small footer or notes.

**Source:** [B1], cell 8, budgeted Hoeffding row; [R1], cell 8, KL row; [R2], cell 13, saved HTML table; [R3], cell 17, saved comparison; [B4] and [R4], cell 17, CUSUM rows. Values here are rounded only for display.

### 4. Benchmarks define what each difference means

**Essential text**

| Scenario | Benchmark | Reported difference |
| --- | --- | --- |
| 1 | True-mean LP over a fixed bid distribution | Expected oracle utility minus realized learner utility: regret |
| 2 | True-mean LP over a fixed super-action distribution | Expected oracle utility minus true expected utility of selected actions: pseudo-regret |
| 3 | Best fixed distribution fitted to the complete realized sequence | Reward-vector difference between oracle and proposed learner distributions: pseudo-regret |
| 4 | One true-mean LP per stationary segment, each capped at $B/T$ | Expected oracle utility minus realized learner utility: gap |

Scenario 2 also stores realized regret and a sampled clairvoyant policy. Scenario 4 also reports a stronger segment-global oracle that can move expected spending between segments.

**Recommended visual:** A benchmark/metric mapping table. Keep “fixed distribution,” “realized sequence” and “segment-local” explicit. Do not label every measure simply “regret.”

**Source:** [R1], cells 4 and 6; [R2], cells 7 and 12; [R3], cells 7 and 12; [R4], cells 8 and 17. In Scenario 3, $x_t$ is the proposed distribution even if a guard changes the executed action. The expected-cost LP constraint and hard realized budget are different constraints.

### 5. Simulation conditions and paired comparisons

**Essential text**

| Scenario | Rounds | Budget | Main trials | Feedback |
| --- | ---: | ---: | ---: | --- |
| 1. Single campaign | 5,000 | 125 | 10 | Selected bid's win outcome |
| 2. Multiple campaigns | 3,000 | 700 | 20 | Semi-bandit, selected arms |
| 3. Primal-dual | 3,000 | 700 | 10 per environment | Full reward/cost vectors after acting |
| 4. Segmented market | 3,000 | 560 | 30 | UCB: semi-bandit. Primal-dual: full feedback |

Within each comparison, baseline and revised policies face the same market paths and use corresponding action seeds. Feedback differs between methods in Scenario 4.

**Recommended visual:** One setup table, with a visible note that these are synthetic simulations. Put full grids, distributions and seed formulas in notes using the snapshot and scenario READMEs.

**Source:** [R1], cells 2 and 8; [R2], cells 2, 3, 5 and 13; [R3], cells 1, 3, 5, 12 and 17; [R4], cells 1, 2, 4, 6 and 15. Seed details: Scenario 1 market seed 20260517 and action seeds $20000+j$; Scenario 2 market seeds $100+j$ and action seeds $1100+j$; Scenario 3 market seeds $17+1000j$ with 100000 added for the changing sequence, PD offset 10000; Scenario 4 market seeds $1000+j$, method offsets 10000/20000/30000. Trial indices start at zero.

### 6. Single campaign: learning from infrequent wins

**Essential text**

One campaign has value 0.6 per win and faces three independent uniform competitors. A bid $b$ wins with probability $b^3$, so low bids produce few wins.

Budget-blind UCB1 and budgeted Hoeffding UCB spend too early. The budgeted policy's loose cost lower bound can make the LP treat a bid as almost free.

The revision uses KL bounds for the Bernoulli win probability and excludes bids above the remaining budget. It keeps the fixed cap $\rho=B/T$.

**Recommended visual:** A bid/observation/estimate/LP decision flow, with the KL confidence bound as the highlighted change. Avoid a dynamic-pacing branch, since this implementation does not have one.

**Source:** [B1], cells 2, 3 and 8; [R1], cells 2–4 and 6. Bids are 0.0, 0.1, …, 0.6. Reward uses $(v-b)p^{UCB}$ and cost uses $bp^{LCB}$ in the revised LP. The baseline stops below remaining budget 1, whereas the revision filters individual bids.

### 7. KL-UCB reduces single-campaign regret

**Essential text**

| Policy | Final regret, mean ± SEM | Mean spend / 125 | Share of LP expected utility |
| --- | ---: | ---: | ---: |
| Budget-blind UCB1 | 40.18 ± 0.49 | 124.29 | 36.4% |
| Budgeted Hoeffding UCB | 39.55 ± 0.70 | 124.38 | 37.4% |
| Budgeted KL-UCB | 11.37 ± 1.50 | 123.20 | 82.0% |

The revised policy obtains more utility with similar total expenditure on these 10 paired trials.

**Recommended visual:** A results table or regret dot plot with saved SEM. Keep expenditure in a separate column or panel. Label regret as expected stochastic-oracle utility minus realized learner utility.

**Source:** [B1], cell 8, saved baseline HTML summary; [R1], cell 8, saved UCB1/KL HTML summary. The oracle LP and its distribution are in [R1], cells 2, 5 and 6. Do not replace this regret with the separately recorded KL pseudo-regret of about 12.26.

### 8. The confidence-bound ablation isolates one decision

**Essential text**

The ablation keeps the revised policy structure and changes only the confidence bound.

| Bound | Mean final regret | Mean spend / 125 |
| --- | ---: | ---: |
| KL | 11.37 | 123.20 |
| Hoeffding | 35.72 | 45.55 |

Wider intervals can also keep the policy choosing low bids that rarely win. The ablation supports KL bounds on this instance.

**Recommended visual:** Two bars for final regret, accompanied by the expenditure table. Identify this as a 10-trial paired ablation that reuses the main KL runs and runs the matched Hoeffding variant. Do not merge the Hoeffding ablation row with the original baseline row on slide 7.

**Source:** [R1], cells 16–17, ablation code and saved HTML table. No published result supports an additional dynamic-pacing experiment in this scenario.

### 9. Multiple campaigns share one spending decision

**Essential text**

Four campaigns share a budget. Some campaigns conflict, so the bidder must choose a feasible combination of bids rather than optimize each campaign independently.

The Combinatorial-UCB baseline spends too early. The revision keeps semi-bandit learning but combines KL reward bounds with empirical expected costs and a cap based on remaining budget and rounds.

$$\rho_t=\frac{B_{\mathrm{remaining}}}{T-t}.$$

**Recommended visual:** The four-campaign conflict chain, plus one example feasible combination and a shared budget. Define $t$ as zero-based in the pacing formula. Use illustrative bids only if clearly labeled as an example, not a recorded action.

**Source:** [R2], cells 2, 3, 5, 7, 10 and 12; [B2], cells 12 and 14. Setup: values (0.95, 0.85, 0.78, 0.90), an 11-point grid on [0,1] filtered by valuation, conflicts 0–1–2–3, 250 feasible super-actions, maximum bid sum 1.8. Cell 2 defines the Beta parameters and 30% common-quantile fraction, cell 3 computes true arm means, and cell 5 generates thresholds. The revised LP restricts support to affordable super-actions.

### 10. Saved checkpoints show the effect of early spending

**Essential text**

| Round | Baseline pseudo-regret / round | Revised pseudo-regret / round |
| ---: | ---: | ---: |
| 750 | 0.0385 | 0.0500 |
| 1,500 | -0.0033 | 0.0349 |
| 2,250 | 0.0819 | 0.0275 |
| 3,000 | 0.1984 | 0.0234 |

The baseline's normalized pseudo-regret rises later in the campaign. The revised policy finishes with a smaller difference from the fixed stochastic oracle.

**Recommended visual:** A two-series marker plot at exactly these four recorded rounds. Straight connecting lines may guide the eye. Do not smooth or interpolate a full trajectory, invent error bands, or silently clip the negative checkpoint.

**Source:** [B2], cell 12, and [R2], cell 13, saved checkpoint summaries. [B2], cell 14, provides the original trajectory plot. A negative partial-horizon value is possible because the comparator has an average-cost constraint, rather than a pathwise optimal action at every round.

### 11. The revised policy uses almost all the budget

**Essential text**

| Policy | Final pseudo-regret | Final realized regret | Mean utility | Mean spend / 700 |
| --- | ---: | ---: | ---: | ---: |
| Combinatorial-UCB baseline | 595.15 | 596.32 | 1047.22 | 698.58 |
| Revised KL-UCB | **70.05** | 73.09 | 1570.45 | 699.84 |

The revised policy obtains 95.6% of the stochastic LP's expected utility. The saved mean pseudo-regret is 70.054544, or **70.1** at one decimal.

**Recommended visual:** A numerical table with pseudo-regret as the main measure and expenditure alongside it. Avoid presenting the sampled clairvoyant policy's realized utility as the LP objective.

**Source:** [R2], cell 13, saved HTML table, corroborated by [B2], cell 12. The stored revised spend is 699.835 and utility is 1570.4475. The reference oracle is in [R2], cell 7. Numerical SEM is not stored in these main tables.

### 12. Confidence and cost ablations support separate changes

**Essential text**

| One-component comparison | Mean final pseudo-regret |
| --- | ---: |
| KL bounds | 70.1 |
| Hoeffding bounds | 100.5 |
| Empirical cost | 70.1 |
| Cost lower confidence bound | 166.2 |

At one quarter of the horizon, mean expenditure is 176.0 with empirical costs and 241.4 with a cost LCB. Each comparison holds the other revised components fixed.

**Recommended visual:** Separate paired panels for the confidence-bound and cost-estimate ablations. A small spending table can support the cost panel. Show 20 paired trials and the saved one-decimal precision. Do not imply that these experiments isolate pacing's individual contribution.

**Source:** [R2], cell 17, printed KL/Hoeffding result; cell 18, printed empirical-cost/LCB result and expenditure at $T/4$.

### 13. Primal-dual control sets a price for spending

**Essential text**

Full feedback makes every feasible combination's reward and cost available after acting. This allows a different control mechanism: Hedge chooses a distribution over combinations, while a dual multiplier raises the penalty for spending.

$$L_t(a)=f_t(a)-\lambda_t(c_t(a)-\rho).$$

The baseline's conservative update rates leave budget unused. The revision calibrates the Hedge and dual rates to their update ranges.

**Recommended visual:** A decision loop connecting Hedge, the executed auction, full feedback and the multiplier update. Keep the Lagrangian visible. Put rate formulas in notes or a compact technical inset only if legible.

**Source:** [B3], cells 6, 8 and 10; [R3], cells 1, 3, 5, 7, 8, 10 and 12. Notes: $K=250$, $\lambda_{\max}=1/\rho$, $G=\max(\rho,C_{\max}-\rho)$, $C_{\max}=1.8$. Revised rates are $\eta_H=\sqrt{8\log(K)/T}$ and $\eta_D=\lambda_{\max}/(G\sqrt T)$. Baseline rates are $\sqrt{\log(K)/T}$ and $1/\sqrt T$. Removing the common term $\lambda_t\rho$ does not change Hedge probabilities at otherwise identical settings. The changing sequence has five sinusoidal cycles with noise standard deviation 0.05 and is generated before play.

### 14. Calibrated rates improve both tested environments

**Essential text**

| Environment | Version | Pseudo-regret / round | Mean realized utility | Mean spend / 700 |
| --- | --- | ---: | ---: | ---: |
| Stochastic | Baseline | 0.249 | 895.8 | 568.6 |
| Stochastic | Calibrated | 0.121 | 1287.9 | 675.8 |
| Changing sequence | Baseline | 0.222 | 835.4 | 557.6 |
| Changing sequence | Calibrated | 0.134 | 1100.0 | 596.8 |

These 10-trial paired comparisons use the best fixed distribution in hindsight. All recorded main runs satisfy the hard budget.

**Recommended visual:** A table grouped by environment. If plotting pseudo-regret, use a dedicated axis and put utility/spend in a separate panel. Do not compare its scale with Scenario 2.

**Source:** [R3], cells 14 and 17. Cell 14 records budget violation 0.00 and replacement frequency 0.000 at displayed precision. The metric uses proposed distributions, while realized utility and expenditure use executed actions. Final numerical SEM is not serialized in the comparison table.

### 15. The guard ablation shows no measured performance gain

**Essential text**

With calibrated rates held fixed, the two guards have identical displayed outcomes over eight paired trials.

| Environment | Guard | Utility | Pseudo-regret / round | Spend / 700 |
| --- | --- | ---: | ---: | ---: |
| Stochastic | Global stop | 1286.3 | 0.121 | 674.5 |
| Stochastic | Per action | 1286.3 | 0.121 | 674.5 |
| Changing sequence | Global stop | 1100.3 | 0.134 | 597.2 |
| Changing sequence | Per action | 1100.3 | 0.134 | 597.2 |

The saved ablation supports rate calibration as the source of the observed improvement on these instances. It does not demonstrate a performance benefit from the guard change.

**Recommended visual:** A matched-row table. Give both guards equal visual weight. Explain in notes that the global guard stops below the maximum round payment, while the per-action guard checks the sampled combination's maximum payment.

**Source:** [R3], cells 18–19. These are separate eight-trial ablation means, not the ten-trial main means on slide 14. “Identical” refers to the saved display precision.

### 16. Primal-dual pseudo-regret across tested horizons

**Essential text**

| Horizon | Stochastic pseudo-regret / round | Changing-sequence pseudo-regret / round |
| ---: | ---: | ---: |
| 100 | 0.2650 | 0.2335 |
| 200 | 0.2638 | 0.2357 |
| 300 | 0.2609 | 0.2315 |
| 500 | 0.2380 | 0.2111 |
| 800 | 0.2042 | 0.1919 |
| 1,200 | 0.1745 | 0.1712 |
| 1,600 | 0.1528 | 0.1547 |
| 2,400 | 0.1299 | 0.1389 |
| 3,000 | 0.1209 | 0.1337 |

Normalized pseudo-regret decreases overall across the tested horizons, with a small initial increase for the changing sequence. Eight trials per horizon keep $B/T$ fixed.

**Recommended visual:** Two marker series on a numerical horizon axis, using exactly the nine recorded x-coordinates. The table above is chart data and need not appear in full on the slide. Do not add a fitted theoretical rate or claim an asymptotic guarantee.

**Source:** [R3], cell 21, printed horizon sweep. The full fixed-policy LP comparator here differs from the scaled comparator in the baseline notebook's adversarial guarantee.

### 17. Changing markets require learning again

**Essential text**

Competitor distributions change across three 1,000-round segments. A bid that was useful earlier may become expensive later, so information has a limited lifetime.

Four campaigns share budget 560. This instance has 27 positive arms and 168 feasible combinations. The number of changes is known by construction.

The main comparator solves one true-mean LP per segment with cap $B/T$. A stronger oracle can redistribute expected spending across segments.

**Recommended visual:** A three-segment timeline with qualitative changes in competitor distributions. Label the local oracle's expected utility as 1228.6 and the segment-global oracle's as 1252.0 only if space permits. These are rounded expected values, not realized pathwise optima.

**Source:** [R4], cells 1, 2, 4, 6 and 8. Campaign values are (0.95, 0.88, 0.78, 0.92), with bid grid (0, 0.10, 0.20, 0.30, 0.40, 0.55, 0.70, 0.85) and the conflict chain. Exact segment Beta parameters are in cell 2 and the snapshot. Do not derive exact gaps by subtracting utility from the one-decimal printed oracle totals.

### 18. Adaptation also needs budget for later segments

**Essential text**

SW-UCB forgets old observations using a calendar-round window. CUSUM-UCB resets an arm's estimate after detecting a change. Both baselines spend too early, limiting what they can learn later.

The revision gives both UCB policies empirical costs and remaining-budget pacing. SW-UCB also widens its window from 219 to 500 rounds. CUSUM's detector settings stay unchanged.

Primal-dual uses calibrated rates but retains its full history. Its baseline already has a per-action affordability check.

**Recommended visual:** Two distinct layers: updating market estimates and controlling expenditure. Show SW and CUSUM as alternative estimation mechanisms feeding the same revised budget LP. Keep full-feedback primal-dual separate.

**Source:** [B4], cells 10 and 12; [R4], cells 2, 10, 12 and 15. The 500-round window uses the known 1,000-round segment length. CUSUM keeps seven reference samples, threshold about 14.63 and exploration probability about 0.0698. Affordability applies to LP support, forced exploration and sampled primal-dual actions.

### 19. CUSUM-UCB has the lowest recorded local-oracle gap

**Essential text**

| Method | Baseline gap ± SEM | Revised gap ± SEM |
| --- | ---: | ---: |
| SW-UCB | 561.99 ± 2.25 | 416.65 ± 2.78 |
| CUSUM-UCB | 533.30 ± 5.10 | 344.93 ± 4.06 |
| Primal-dual, full feedback | 715.78 ± 2.55 | 660.35 ± 2.77 |

Each gap subtracts realized learner utility from the same segment-local oracle's expected utility. These are 30-trial paired means. The methods do not all receive the same feedback.

**Recommended visual:** Grouped dots or bars with numerical SEM, clearly labeled “Final gap to segment-local oracle.” All series on this slide share that benchmark. Do not relabel these gaps as Scenario 3 pseudo-regret.

**Source:** [B4] and [R4], cell 17, saved HTML tables. [R4], cell 18, separately stores revised gap/round values 0.1389, 0.1150 and 0.2201. The secondary segment-global gaps are in cell 17 and the snapshot, but should not mix with local gaps in the chart.

### 20. Window width and expenditure remain distinct effects

**Essential text**

The paired SW-UCB ablation keeps empirical costs and pacing fixed. A 500-round window gives local gap 416.6, compared with 496.1 for 219 rounds.

| Method | Baseline spend / 560 | Revised spend / 560 |
| --- | ---: | ---: |
| SW-UCB | 559.98 | 559.81 |
| CUSUM-UCB | 559.97 | 559.60 |
| Primal-dual | 393.41 | 348.92 |

Primal-dual improves utility while spending less, leaving 211.08 unused. CUSUM's mean reset count rises from 18.2 to 32.0, but reset counts do not measure detection accuracy.

**Recommended visual:** A small window-ablation comparison and a separate expenditure table. Put the CUSUM diagnostic in a note if the slide becomes crowded. Neither a reset target nor a windowed primal-dual result should appear.

**Source:** [R4], cells 19–20, 30-trial paired window ablation; [B4] and [R4], cell 17, expenditure and utility; [B4], cell 20, and [R4], cell 22, reset diagnostics. The comparison count 54 equals two changes times 27 arms and is not a detection-accuracy target. The experiments do not isolate a single cause of primal-dual's remaining gap.

### 21. What the evidence supports

**Essential text**

KL bounds improve the saved sparse-win comparisons. Empirical cost estimates improve the combinatorial cost ablation. Calibrated primal-dual rates improve the tested full-feedback environments, while the guard ablation shows no measured gain. A wider SW-UCB window improves the selected segmented instance.

The conclusions are limited to synthetic auction configurations and the available saved outputs. Joint changes do not identify every component's contribution. Finite horizon sweeps do not prove asymptotic behavior, and feedback and benchmark differences constrain comparisons.

**Recommended visual:** A short evidence/conclusion table, pairing each supported claim with its ablation or comparison. Keep limitations readable rather than in a fine-print disclaimer. Do not include results for Scenario 1 dynamic pacing or Scenario 4 windowed primal-dual. Any later implementation of these ideas would be future work, not an existing experiment.

**Source:** [R1], cells 16–17; [R2], cells 17–18; [R3], cells 17–21; [R4], cells 19–22. Scope and unavailable evidence are recorded in the [documentation audit](docs/documentation-audit.md).

### 22. Code, saved outputs and sources

**Essential text**

Repository: [antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning)

Eight notebooks document the baselines and revisions. Scenario READMEs explain configurations, paired comparisons and limitations.

Evidence reference: `38a050b32a6bd757c0c2c4db18c029caa039e919`.

Antonio Arbizzani Tonelli, Leonardo Arisi, Claudia Berra, Gaia Di Paolo.

**Recommended visual:** A readable repository link, notebook guide and source-map link. A QR code may encode the repository URL if desired. Do not reuse the old unhyphenated GitHub profile address.

**Source:** [Notebook guide](notebooks/README.md), [source map](docs/presentation-sources.md), [numerical snapshot](docs/presentation-data.json), and [reference commit](https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/commit/38a050b32a6bd757c0c2c4db18c029caa039e919). This is a documentation/source slide, with no additional empirical claim.

## Prompt for Claude Design

Copy the prompt below and attach this document, `docs/presentation-sources.md` and `docs/presentation-data.json`. The legacy PowerPoint can be attached as background, but it is not the numerical authority. Attach the notebooks too if source inspection is available. Ask for a PPTX and a PDF only when ready to produce the redesigned presentation.

```text
Create a professional English presentation titled “Adaptive Budget Allocation for Online Advertising” for a technical portfolio and interview discussion.

Use the attached PRESENTATION_DRAFT.md as the content brief. Follow its 22-slide sequence, including the technical backup slides. Each slide specifies essential text, a recommended visual and exact source cells. Use docs/presentation-sources.md and docs/presentation-data.json for provenance and chart inputs. The evidence reference is commit 38a050b32a6bd757c0c2c4db18c029caa039e919. The legacy PowerPoint is background only and contains outdated claims.

Tell the story through the budget decision. Open with choosing bids while preserving money for later auctions. Explain why sparse wins, campaign conflicts, full feedback and changing markets call for different approaches. For each scenario connect the practical problem, baseline, observed limitation, revision and recorded result. Avoid presenting a catalogue of algorithms or university exercises. Use sober, natural English and preserve all four authors' names.

Design a clear 16:9 deck with generous space, strong typographic hierarchy and consistent baseline/revised colors. Use concise titles, a small amount of text per slide, legible mathematical notation and editable charts/tables where supported. Put detailed setups and technical qualifications in speaker notes, retaining visible benchmark, sample-size and metric labels near each result. Give every empirical figure and table a compact source footer plus precise notebook/cell references in notes. Cell indices are zero-based and include markdown cells. Do not shrink dense tables to unreadable sizes. The recommendations may be simplified visually without changing the comparisons or evidence.

Use only saved values in the attachments. Scenario 2 final mean pseudo-regret is 70.054544, displayed as 70.05 or 70.1. Its revised mean spend is 699.835/700. Its ablations are KL 70.1 versus Hoeffding 100.5, and empirical-cost 70.1 versus cost-LCB 166.2. At T/4, spend is 176.0 versus 241.4. Do not reuse approximately 72, 101, 165 or 177 from the legacy deck.

Keep regret, pseudo-regret and oracle gap distinct. Scenarios 1–2 use stochastic true-mean LPs, Scenario 3 uses the best fixed distribution fitted to the realized sequence, and Scenario 4 uses a segment-local true-mean LP. Its stronger segment-global oracle is a separate comparator. Never rank results across these benchmarks or put their numerical values on a common performance axis. The sampled clairvoyant policy is different from the expected LP objective. Scenario 3 pseudo-regret uses proposed distributions, while actual utility and spending use executed actions.

Do not attribute a performance benefit to the Scenario 3 guard change. Its eight-trial ablation gives identical displayed outcomes. In Scenario 4 the primal-dual baseline already has the per-action guard, and the revision improves utility while spending less. UCB has semi-bandit feedback there, while primal-dual has full feedback. The 500-round SW window uses the known 1,000-round segment length. CUSUM reset counts are diagnostics, not detection accuracy.

Exclude results for dynamic pacing in Scenario 1 and windowed primal-dual in Scenario 4. No implementation and saved output are available for those experiments. Do not invent metrics, error bars, time-series observations, asymptotic guarantees or real-campaign claims. Do not run experiments or modify notebook code. Proposed future work must be explicitly labeled as unimplemented and unevaluated.

For the Scenario 2 checkpoint figure, plot exactly four recorded rounds with markers and optional straight connecting lines. Keep the negative baseline checkpoint. For the horizon figure, use the actual numerical horizon coordinates. Do not create smooth trajectories from aggregates. Use saved SEM only where provided, and label it as SEM rather than a confidence interval. Keep the saved display precision. Recreate summary charts with English labels rather than pasting legacy images with Italian labels. Conceptual visuals must not look like measured results.

Use this repository URL throughout:
https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning

Preserve the legacy files. Name the redesigned deliverables Adaptive-Budget-Allocation-for-Online-Advertising-Redesigned.pptx and the matching .pdf. Generate the PDF from the finished PPTX. Before delivery, inspect every slide and PDF page for clipped text, overlapping labels, equation rendering, chart values, consistent axes and readable citations. Report any unavailable source or unsupported claim instead of filling a gap with an invented result.
```

## Pinned notebook references

The short source labels used above resolve to these versioned notebooks. They are also listed in the [source catalogue](docs/presentation-sources.md).

- [B1]: single-campaign baseline.
- [R1]: single-campaign KL revision.
- [B2]: combinatorial baseline.
- [R2]: combinatorial KL, empirical-cost and pacing revision.
- [B3]: primal-dual baseline.
- [R3]: calibrated primal-dual revision.
- [B4]: adaptive-learner baseline.
- [R4]: segmented-market revision.

[B1]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/01_single_campaign/baseline_ucb.ipynb
[R1]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/01_single_campaign/budget_aware_kl_ucb.ipynb
[B2]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/02_combinatorial_campaigns/baseline_combinatorial_ucb.ipynb
[R2]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/02_combinatorial_campaigns/dynamic_budget_pacing.ipynb
[B3]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/03_primal_dual_control/baseline_primal_dual.ipynb
[R3]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/03_primal_dual_control/calibrated_primal_dual.ipynb
[B4]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/04_non_stationary_market/baseline_adaptive_learners.ipynb
[R4]: https://github.com/antonio-arbizzani-tonelli/adaptive-budget-allocation-online-learning/blob/38a050b32a6bd757c0c2c4db18c029caa039e919/notebooks/04_non_stationary_market/change_detection_and_pacing.ipynb
