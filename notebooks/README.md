# Notebook Guide

The notebooks follow one continuous investigation into budget-constrained bidding. The action space and the uncertainty of the auction environment become progressively more demanding, while the central objective remains unchanged: learn a profitable policy without exhausting the budget before the horizon.

Each stage contains a baseline and a revised learner. The baseline establishes the behavior of a standard method on the selected instance. The revised notebook diagnoses the observed limitation and changes only the components needed to address it. Comparisons use the same environment realizations and seeds whenever possible.

## Project stages

- [`01_single_campaign`](01_single_campaign): learning a bid from rare wins while enforcing a hard campaign budget;
- [`02_combinatorial_campaigns`](02_combinatorial_campaigns): coordinating several campaigns through feasible super-actions and a shared budget;
- [`03_primal_dual_control`](03_primal_dual_control): using a dual variable as an endogenous price for budget consumption;
- [`04_non_stationary_market`](04_non_stationary_market): preserving the ability to relearn when competitor behavior changes.

Return to the [project overview](../README.md) for the complete narrative and cross-stage results.
