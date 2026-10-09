# Conditional second-call v1 development diagnostic

Frozen A/B/C experiment completed without parameter changes after smoke. 722 development molecules; no confirmation reads, external API calls, provider forward passes, or GPU work.

| Method | Utility / input | Calls | Wrong | Refused | Coverage |
|---|---:|---:|---:|---:|---:|
| stop_only | -0.20252078 | 722 | 60 | 316 | 56.2327% |
| confidence_cascade | -0.19594183 | 897 | 74 | 234 | 67.5900% |
| pair_mean_gain | -0.20286704 | 872 | 66 | 287 | 60.2493% |
| conditional_gain | -0.20407202 | 884 | 65 | 294 | 59.2798% |

Conditional-gain paired differences:
- versus stop_only: -0.00155125, 95% CI [-0.00385446, +0.00777638].
- versus confidence_cascade: -0.00813019, 95% CI [-0.01405972, +0.00334978].
- versus pair_mean_gain: -0.00120499, 95% CI [-0.00349674, +0.00270366].

Current fixed construction fails the prespecified gate. The strongest implemented legal control is confidence_cascade. All12 orderedpairs have B support in everyfold. The privileged offline DP utility bound is -0.15351801; unconstrained positive gain per input is 0.05398892. This establishes remaining opportunity under the fixed verifier, not learnability or a deployable result.

The interval uses10,000 fold-internal scaffold block draws, sampled with replacement and concatenated in draw order, preserving original episode order within each block. Every method reruns its causal token controller in every replicate. Models stay fixed; intervals omit training uncertainty. Bootstrap reordering can shift its mean from the original-order point estimate. Repeatedly inspected development data cannot be relabeled untouched confirmation.

Selector provenance: original A-only fit function was executed in isolation with A-index-guarded cached features/probabilities; cached weights matched within1e-12. Providers have separate upstream validation-selected checkpoints with zero canonical-SMILES overlap with these development molecules; see selector_provenance_audit.json.

13 focused tests passed. Smoke wall time0.936s; full wall time5.576s. No failed experiment attempts occurred. Tests are synthetic contract checks, not scientific effect evidence.

## Artifacts

- `tests_attempt01/`: pre-execution test config/source hashes, stdout and result.
- `smoke_v1/` and `full_v1/`: pre-computation config, source/card snapshots, execution trace, model weights and source hashes.
- `full_v1/A_verifier_training.jsonl`: each weighted A subset input.
- `full_v1/B_paired_targets.jsonl`: every offline paired training target and feature.
- `full_v1/policy_decisions.jsonl`: legal visible evidence and actions, without truths or unselected outputs.
- `full_v1/C_evaluator_panel.jsonl`, `evaluator_outcomes.jsonl`: privileged diagnostics and per-method evaluation, separate from policy evidence.
- `full_v1/bootstrap_fold*_block_draws.npy`, block maps, `bootstrap_results.jsonl`, `bootstrap_utilities.npy`: exact10000 draws and results.
- `full_v1/artifact_manifest.json`: SHA256 of run artifacts.

## Reuse

Reusable assets are the observed-only policy functions, paired gain targets with frozen-verifier provenance, exact prefix-budget controller/DP, matched no-confidence ablation and saved scaffold bootstrap replay. The negative result motivates returning to representation/target learnability; it does not authorize tuning this frozen run or escalating to online/agent claims.

Runner SHA256: `09832518ef374a55a06f27926b444035c296670c463c3821b081265934e9f20a`.
Card SHA256: `53a455ece5022f9aab678392a375b40837be4faf65f3de4bf4f670d819ceeee9`.
