# Independent primary arrival audit

{
  "status": "verified",
  "phase": "full",
  "run_path": "/Users/jingwu/work/benchmark/experiments/logs/task49_primary_arrival_robustness_20261008/full_v1",
  "run_config_sha256": "7d2b3985522a5a7822fab40270ca1866afafbedefadfcded6216697f836674d4",
  "run_manifest_sha256": "c3ff33a5923de655f6a900afbfbdbf196c49909a3aa9d507e83dce0def8b88f2",
  "counts": {
    "streams": 61440,
    "decisions": 9646080,
    "score_values": 38584320,
    "final_numeric_states": 61440,
    "full_instance_states": 61440,
    "rng_states": 23040,
    "legacy_hashes": 61440,
    "old_main_streams": 0,
    "old_credit_streams": 0,
    "old_exact_final_hashes": 0,
    "old_credit_algebraic_final_hashes": 0,
    "state_arrays": 422400
  },
  "maximum_absolute_errors": {
    "scores": 0.0,
    "states": 0.0,
    "original_scores": 0.0,
    "metrics": 0.0
  },
  "mutation_checks": [
    {
      "mutation": "malformed_release",
      "rejected": true,
      "reason": "stream release_tick exact"
    },
    {
      "mutation": "terminal_label",
      "rejected": true,
      "reason": "stream returned_label exact"
    }
  ],
  "resources": {
    "wall_seconds": 511.025249083,
    "cpu_seconds": 510.798452,
    "peak_rss_bytes": 578682880
  },
  "projected_full_auditor_cpu_seconds_with_25pct_margin": null,
  "full_auditor_projection_within_1800_seconds": null,
  "scope": "All receipt fields, every full final numerical/instance state, input/source hashes, RNG and integer counts independently checked. No provider or runner imports.",
  "interpretation": "Uniform orders of fixed viewed cohort; two fixed policy seeds; mean MC uncertainty only; not scaffold/deployment uncertainty.",
  "representation_notes": [
    "Main Frozen logits transformed by clipped sigmoid only for mechanism-v2 Frozen comparison.",
    "Main Local direct counts and mechanism-v2 accumulated residual compared within atol=rtol=1e-10; all actions exact.",
    "Legacy tail timestamps after last decision allowed to differ; update sequence and final commitments checked."
  ]
}
