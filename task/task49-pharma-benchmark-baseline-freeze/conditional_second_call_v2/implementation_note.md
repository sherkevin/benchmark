# Fusion diagnostic v1 implementation note

- Frozen card: `fusion_diagnostic_card_v1.md`, SHA `b0da0f3f72fbe08d3beacd8073a2f2754f38554d67283fe387e2548aee9fc319`.
- Runner: `src/probe_headless/analyze_conditional_fusion_diagnostic_v1.py`.
- Independent verifier: `verify_fusion_diagnostic_v1.py` in this directory.
- Real command:

```bash
python3 src/probe_headless/analyze_conditional_fusion_diagnostic_v1.py \
  --go --card-hash b0da0f3f72fbe08d3beacd8073a2f2754f38554d67283fe387e2548aee9fc319 \
  --output experiments/logs/task49_conditional_fusion_diagnostic_20261008/real_v6
python3 task/task49-pharma-benchmark-baseline-freeze/conditional_second_call_v2/verify_fusion_diagnostic_v1.py
```

The final runner reads only `B_paired_targets.jsonl`, `B_features_and_targets.jsonl`, the two artifact manifests, and the two sets of five frozen per-fold model metadata files. It validates manifest/model lineage, writes an input manifest before analysis, rejects a missing explicit `--go` or card hash mismatch, and writes S/U rows, complete predeclared groups, pair correlations, JSONL outputs, code/commit provenance, and a resource receipt. It does not call the legacy input loader and does not read C outcome, policy, summary, bootstrap, provider, API, GPU, or confirmation inputs.

The independent verifier rebuilds native outputs and all 12 ordered pairs from the v1 B receipt and frozen verifier coefficients. It checks the selected branch evidence and output row counts. The diagnostic is descriptive and fold-local; it supplies no confidence interval, significance claim, or method adoption.

The first four real runs are preserved as invalid implementation stages; they were not used for the branch decision. `real_v6` is the only accepted execution after the contract review findings were repaired.

Independent contract review: `implementation_independent_review.md`. Its ten findings were repaired before real_v6; the prior outputs remain for audit.
