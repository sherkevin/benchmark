# Baseline semantics revision v1

Purpose: define the stopped MIRA-Relative negative control in the paper so the comparator table is self-contained.

Change: add one sentence under Baselines and fairness identifying MIRA-Relative as a global reliability-factor ablation, retained as a negative control and not a second proposed method.

No numerical result, method claim, or experiment configuration changes.
Acceptance: submission remains 9 pages total (8 body + references), with the same warning gate.

Implementation audit correction: `MIRA-Relative` uses one context-independent multiplicative factor per provider (`rho_a`), not one scalar shared across all providers. The paper sentence was corrected before submission.
