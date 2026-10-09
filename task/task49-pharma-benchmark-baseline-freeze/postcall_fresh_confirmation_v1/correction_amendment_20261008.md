# Post-call extension correction amendment (2026-10-08)

## Why this amendment exists

The original v1 report and the first paper binding contained two interpretation errors that must remain visible in the evidence history:

1. The confidence-refuse minus same-coverage-random scaffold bootstrap interval was copied from the confidence-refuse minus always-accept comparison. The correct intervals are `[+2.77,+9.29]` percentage points for random and `[+3.34,+9.72]` points for always-accept.
2. The 157 confirmation molecules were already used in earlier holdout routing work before this post-call analysis. They are scaffold-disjoint from the development fit/validation rows, but they are not an untouched prospective confirmation cohort. This extension is therefore a secondary/post-hoc analysis on an existing held-out cohort. The provider predictions and policy thresholds were frozen before this replay, and no policy or provider was changed after seeing the extension result; that does not restore prospective E6 status.

A third scope clarification is also required: `same-coverage random` is an evaluator-side conditional null. The runner first observes the confidence gate's accepted count and then samples exactly that many positions. It is useful for the paired selection comparison, but it is not an implementable online policy and does not establish seed robustness.

## Required corrections

- Preserve the original v1 files and this amendment; do not overwrite evidence history or delete the old wording.
- Mark E6 prospective-fresh-confirmation as **not met** for this extension. The result can support a secondary post-call risk-gating observation only.
- Recompute and archive utility uncertainty with scaffold-cluster bootstrap. The fixed utility point remains descriptive until this audit is complete.
- Run the strengthened independent verifier with an explicit expected-row count and mutation coverage for truncation, duplicate keys, probabilities, calls/status, labels, and fit artifacts.
- In the paper, call the result `post-call secondary analysis on an existing held-out cohort`; use the corrected random interval and define the utility. Do not describe it as a new untouched confirmation, a selector gain, an online update, or an escalation result.

## Status

This amendment is a correction to interpretation and evidence binding. It does not rerun providers, alter thresholds, alter splits, or tune a policy. The original report snapshots, result, and receipts are preserved. The old verifier remains available in Git history; the strengthened implementation is versioned in place. Corrected statistical outputs are written under `analysis_audit_v3`, receipt checks under `strict_audit_v3`, and the corrected paper source is built separately.
