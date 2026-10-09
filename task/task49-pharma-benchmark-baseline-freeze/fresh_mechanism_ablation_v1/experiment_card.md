# Fresh mechanism ablation — experiment card (post hoc; withdrawn as a mechanism card)

> **Correction.** This card was written after both executions and is retained
> for provenance only. It is not a preregistration. The implemented
> Shared-Credit arm has a provider-common additive term that cancels in its
> `argmax`, and its initial ranking differs from Frozen. The paired
> Local--Shared comparison therefore does not isolate selected-provider
> credit. See `report.md` for the authoritative interpretation.

## Question

Does delayed selected-only feedback help because it is attached to the
provider that was actually selected, or is any pooled feedback signal enough?
This is the missing mechanism test behind M5/M13. The four fresh providers are
singleton families, so this card does not claim to identify family sharing.

## Matched arms

All arms receive the same molecule order, four-provider menu, Morgan64
selector feature, sealed fold prior, one provider call, terminal contract, and
release schedule $D\in\{0,1,4\}$. The control arms are:

1. **Frozen:** no update after release;
2. **MIRA-Local:** the main provider-local residual policy;
3. **MIRA-Shared:** one posterior over all released selected rewards, with each
   provider retaining its fit-rate anchor. In the implementation the pooled
   posterior is a common additive term, so this arm is static recentering
   rather than a matched wrong-credit online control;
4. **MIRA-Relative:** existing provider-wise multiplicative negative control;
5. matched Online-SGD, Greedy LinUCB, and BayesianRouter as modern online
   comparators.

The local and shared arms have the same prior count (20), feedback visibility,
delay queue, and state-update timing. No arm reads current truth or an
unselected provider output.

## Originally planned measurements (not preregistered)

For every method and matched fold/seed/delay stream, report pooled correctness
difference versus Frozen, action changes, rescued decisions, harmed decisions,
and policy update p95. Confidence intervals use 10,000 deterministic bootstrap
draws over the 124 Bemis--Murcko scaffold clusters (seed 20261007), not event
rows. Run both the natural label stream and the already registered
midpoint-selected-fit-best label-flip stress.

## Originally proposed falsification (not applied)

This criterion is withdrawn because the Shared arm does not vary only the
credit-assignment factor. A null natural result is not a safety or equivalence
proof.

## Data provenance

The CPU replay reads the sealed provider prediction receipt, confirmation
manifest, and fit-prior cache. It does not rerun provider inference or call an
external API. Raw JSONL, config, summary and derived analysis remain separate;
the original fresh confirmation receipts are unchanged.
