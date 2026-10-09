# Matched SGD full development run plan

- Project: `damo_genomic_gene`
- Queue: `damo_genomic_gene_a800`
- Algorithm: `pytorch280`
- Workers: 1; GPU: 1 × A800
- Entry: `src/probe_headless/nebula_matched_sgd_diagnostic.py`
- Input: `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- Input SHA-256: `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- Planned parameters: `--folds 5 --seeds 0,1 --delays 0,1,4 --epochs 200`
- Expected streams/events: 90 streams; 12,996 decisions; 12,996 feedback; 10 fit snapshots; 2,000 epochs.

The replayable-evidence smoke with the same entry and recovery path passed before this run. Submit exactly one task with a pinned commit. Download all logs after completion, sanitize them, then run `recover_pharma_matched_sgd_diagnostic.py`. No scientific result is accepted if recovery fails.
