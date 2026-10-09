# Two-representation diagnostic evidence

Purpose: test whether normalized chiral Morgan1024 improves selection against a zero-padded Morgan64 control and the existing Corrected Frozen. Both networks and initial states are shared; effective capacity is not matched. The frozen card/review live under `task/task49-pharma-benchmark-baseline-freeze/fresh_representation_v1/`.

`implementation_checks_v1/` records 15 synthetic checks. `input_v1/` records CPU preparation, source hashes, ten expanded initial states and the frozen 722-molecule cohort. No provider/LLM inference or confirmation access occurs. `input_independent_check_v1/` independently rebuilds both representations and checks inherited rows and hashes.

The 34,587,591-byte input is stored as lossless `input_v1/inputs.json.gz`; the expanded JSON is local and travels through OSS. Restore before rerunning:

```bash
gzip -dc experiments/logs/task49_fresh_representation_20261007/input_v1/inputs.json.gz > experiments/logs/task49_fresh_representation_20261007/input_v1/inputs.json
```

Only run restoration if the target is absent. Expected raw SHA256: `8109405b121472e7628f987ccae72ee90d7442f64f36301efca65c733ce58447`. Compression metadata proves byte-identical roundtrip. Old shared-initial source checkpoints are recovered from the previous learned-prior archive as described in that run's README.

`remote_v1/` records preflight and subsequent platform operations. A task-list timeout is a transport observation, not a training failure. No scientific result is claimed before complete output recovery and independent verification.

Completed: single task01a1153c076660894f3390124224a569, commit84c61207, success/FINISHED; remote_output_v1/outputs.tar.gz contains the full40-file manifest and30checkpoints. verification_v1/verification.json independently verified all20fits/2888OOF. Representation upgrade rule failed; see fresh_representation_v1/report.md. No task remains running.
