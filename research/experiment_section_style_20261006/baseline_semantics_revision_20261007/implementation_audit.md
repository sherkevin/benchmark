# MIRA-Relative implementation audit

- Date: 2026-10-07
- Scope: semantic check for the comparator sentence in `paper/aamas2027/main.tex`.
- Receipt: `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`.
- Runner config declares source commit `f48e1d45`; the runner and policy blobs match that commit.

## Exact update

`mira_relative_reliability.py` computes, for provider $a$,

$$
\hat e_a=\frac{n_{\min}\tau_{\mathrm{fail}}+n_{\mathrm{fail},a}}
                 {n_{\min}+n_{\mathrm{valid},a}},\qquad
\rho_a=\min\left(1,\frac{1-\hat e_a}{1-\tau_{\mathrm{fail}}}\right),
$$

and scores the provider with $q_a^0(x)\rho_a$. The factor is one scalar per
provider, independent of the current molecule at a fixed released state; it
changes only through selected-only released history. It is therefore not one
scalar shared by all providers. A shared scalar would preserve the argmax.

With the fresh configuration $n_{\min}=20$ and $\tau_{\mathrm{fail}}=0.20$,
cold start has $\rho_a=1$. A released selected error gives
$\rho_a=20/21$, while a selected success does not increase the factor above
one. Runtime failure is handled in a separate availability streak; chemical
correctness does not hard-block an arm in this stopped control. Unknown labels
do not enter the fresh scored stream.

## Receipt cross-check

The 30 Frozen/Relative stream pairs contain 76 provider action changes in both
the v1 and v2 fresh receipts, with a maximum of 10 changes in one stream. The
pooled Relative and Frozen correctness is nevertheless identical at 89.81%:
the changes produce zero net rescued/harmed correctness difference on this
natural panel. The current paper therefore describes Relative as a negative
control for aggregate improvement, while allowing that its provider-wise
factors can change rankings. The 1,656 action changes and 82/82 rescued/harmed
counts belong to MIRA-Relay, not to Relative.

This audit corrects an older report sentence claiming identical Relative/Frozen
action sequences; that sentence is not used as a current paper claim.
