# Matched SGD A800 smoke run plan

- Project: `damo_genomic_gene`
- Queue: `damo_genomic_gene_a800`
- Algorithm: `pytorch280`
- Workers: 1
- GPU: 1 × A800
- Entry: `src/probe_headless/nebula_matched_sgd_diagnostic.py`
- Commit: `a83e7d7c511b0c8c533bc51ed2d87c446da8c838`
- Input: `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- User params: `--input ... --input-sha256 a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf --code-commit a83e7d7c511b0c8c533bc51ed2d87c446da8c838 --folds 1 --seeds 0 --delays 0 --epochs 2`

先保存 wrapper dry-run 和标准运行契约，再提交一条任务。只要 smoke 未成功，不提交完整 development 诊断。
