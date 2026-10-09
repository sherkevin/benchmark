# Independent primary arrival audit

{
  "status": "verified",
  "phase": "smoke",
  "run_path": "/Users/jingwu/work/benchmark/experiments/logs/task49_primary_arrival_robustness_20261008/smoke_v2",
  "run_config_sha256": "34002ef51e61b79331dd6ed846b69f675b123f04949850c24b401ca72e0d93e7",
  "run_manifest_sha256": "082b6784907a009b87117779b72fef6faad7de1bf6ab2f17654f06ae677aa00a",
  "counts": {
    "streams": 480,
    "decisions": 75360,
    "score_values": 301440,
    "final_numeric_states": 480,
    "full_instance_states": 480,
    "rng_states": 180,
    "legacy_hashes": 480,
    "old_main_streams": 420,
    "old_credit_streams": 180,
    "old_exact_final_hashes": 480,
    "old_credit_algebraic_final_hashes": 120,
    "state_arrays": 3300
  },
  "maximum_absolute_errors": {
    "scores": 0.0,
    "states": 0.0,
    "original_scores": 2.220446049250313e-16,
    "metrics": 1.4210854715202004e-14
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
    "wall_seconds": 5.66031025,
    "cpu_seconds": 9.212507,
    "peak_rss_bytes": 497500160
  },
  "projected_full_auditor_cpu_seconds_with_25pct_margin": 630.9983999999997,
  "full_auditor_projection_within_1800_seconds": true,
  "scope": "All receipt fields, every full final numerical/instance state, input/source hashes, RNG and integer counts independently checked. No provider or runner imports.",
  "interpretation": "Uniform orders of fixed viewed cohort; two fixed policy seeds; mean MC uncertainty only; not scaffold/deployment uncertainty.",
  "representation_notes": [
    "Main Frozen logits transformed by clipped sigmoid only for mechanism-v2 Frozen comparison.",
    "Main Local direct counts and mechanism-v2 accumulated residual compared within atol=rtol=1e-10; all actions exact.",
    "Legacy tail timestamps after last decision allowed to differ; update sequence and final commitments checked."
  ]
}
