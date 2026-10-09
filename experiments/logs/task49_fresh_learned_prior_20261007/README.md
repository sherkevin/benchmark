# Four-provider learned-prior evidence (2026-10-07)

`input_v1/inputs.json` is the sealed 722×4 development input. `prepare_v1/` contains the 8 synthetic tests. `remote_v1/` contains OSS checks and the single Nebula task (`01a114f6d306db0a283dce1d4453aa33`), pinned to commit `86543eca491814a19634529e27237d80f04083c4`. A failed status read did not trigger a second submission.

`remote_output_v1/outputs.tar.gz` is the complete, immutable worker output (1,420,185 bytes), SHA256 `2ac16485c3784976521e13b06dc000e5c9321d3efa084e7fe200af16bcdfe3bf`. It contains 30 state_dict checkpoints, 4,000 epoch records, 2,888 OOF predictions, input/configuration and manifests. The 41 archive members were safely extracted and 40 manifest-listed file sizes/hashes verified. Extracted originals remain local; Git keeps the archive to avoid duplicated raw files.

To restore in a fresh clone:

```bash
mkdir -p experiments/logs/task49_fresh_learned_prior_20261007/remote_output_v1/extracted
tar -xzf experiments/logs/task49_fresh_learned_prior_20261007/remote_output_v1/outputs.tar.gz -C experiments/logs/task49_fresh_learned_prior_20261007/remote_output_v1/extracted
.srcprobe/beir_venv/bin/python -B -m src.probe_headless.verify_fresh_learned_prior_v1 --input experiments/logs/task49_fresh_learned_prior_20261007/input_v1/inputs.json --input-sha256 33c9d092f840e4f2c75eec562271abe00891de0362a6ec3a8ad9c78a7aeb1cfc --results experiments/logs/task49_fresh_learned_prior_20261007/remote_output_v1/extracted/training --output /tmp/fresh_prior_independent_check_new
```

Use a new verifier output directory. The verifier expects the sealed source files at the submitted revision; preserve or restore that revision in a separate checkout if subsequent code has changed. `.srcprobe` is the existing local Python environment; a portable environment needs Python3.10+, NumPy and PyTorch with `weights_only=True` checkpoint support. CUDA is not needed for independent NumPy forward verification.

`verification_v1/verification.json` independently checks source and model identity, scores, choices, validation-best selection from logged counts, and all paired/LOO/go quantities. It does not rerun 4,000 optimizer steps or independently remeasure GPU timing. Two seeds share 722 molecules. Both BCE+CLS and BT+CLS fail the predeclared upgrade rule; see the Task49 `fresh_learned_prior_v1/report.md`.

No credential values are stored. Raw CLI progress whitespace is intentionally preserved. `remote_v1/roundtrip_inputs.json` is only a local transport copy of the already committed sealed input.
