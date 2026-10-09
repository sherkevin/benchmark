# Fresh MIRA full-state replay

## Result

The import-free checker independently reconstructed the sealed prior weights,
Morgan features, provider scores, selected actions, selected-only counters,
version increments, and delayed lifecycle for both existing receipts.

| receipt | MIRA streams | decisions | feedback transitions | state-hash checks | max score error |
|---|---:|---:|---:|---:|---:|
| natural fresh v2 | 30 | 4,710 | 4,710 | 9,420 | 0 |
| controlled drift v2 | 30 | 4,710 | 4,710 | 9,420 | 0 |

Each receipt also yields 14,130 canonical event rows (decision, terminal and
feedback). Every available production hash on decision and feedback records
matched the independently reconstructed full state. Terminal records have no
hash in the historical schema and were checked through their immutable
lifecycle binding. The receipt summary has no final-state hash, so
`production_state_hash_match` is recorded as unavailable rather than false.

## Integrity checks

- A copied receipt with one reward changed was rejected at the corresponding
  reward/terminal consistency check.
- A copied receipt with one score changed by `0.001` was rejected at the score
  check.
- The verifier imports no production implementation. It recomputes the
  feature vector and score surface from the sealed manifest and prior cache.
- No new provider, LLM, or GPU call was made.

## Evidence

- Checker: `src/probe_headless/verify_fresh_context_replay_v2.py`, SHA256
  `1ad88223e41af4ee1a84803558a3466db70922b7f54f7a56c6d8a2343a4065f6`.
- Natural verification: `experiments/logs/task49_fresh_context_replay_20261007/natural_v8/verification.json`, SHA256
  `9c3315fd2d0d8d6cd6107214cbb84af25ec00784d9ef2a03bbfbec3a42bb6eec`.
- Controlled verification: `experiments/logs/task49_fresh_context_replay_20261007/stress_v5/verification.json`, SHA256
  `9a0771a9e563321adf637c1bcaf083570b2f4a983a17792fa60394703649d627`.
- Natural canonical full-state trace: SHA256
  `47ad67d13b0f40e64ab3074e1eb790d25e374a0c35f85da0c6ea1e3dcb25ccaa`.
- Controlled canonical full-state trace: SHA256
  `8a01cd4e66e595995d56820af6893a581615af8337d7b1f9c0596f6b2e58b121`.

## Gate interpretation

This closes the replay evidence gap for the current fresh implementation more
strongly than the earlier dynamic-only replay: the immutable prior and the
dynamic state are both reconstructed independently, and the production hashes
are checked event by event. It does not close the mechanism-ablation gap. The
current four-provider roster still cannot identify family sharing versus
provider-local residuals, and it does not supply interface-null or remove-state
controls. The methods gate therefore remains HOLD pending a matched mechanism
matrix.

## Boundary

The sealed fit cache remains an input; the offline optimizer is not rerun. The
provider predictions, terminal outputs, and evaluator rewards remain receipt
inputs. Future fresh runs should record initial feature/weight bytes and a
final-state hash directly so that these two remaining audit boundaries are
removed from the receipt contract.
