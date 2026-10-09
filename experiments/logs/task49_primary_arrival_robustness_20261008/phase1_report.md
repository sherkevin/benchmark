# Primary arrival robustness: phase one

The runner directly executes the original main-table policies and the existing fixed-permutation credit policy. No provider forward pass, API, offline refit, or GPU was used. The frozen experiment card governs 480 streams per order. Full random-order execution remains gated on independent verification and the coordinator's resource GO.

## Engineering checks

- `tests_v1`: 8 tests passed; `tests_v2`: 10 tests passed; `tests_v3`: 11 tests passed.
- `smoke_v1`: all 480 streams completed; interrupted after 112.47 s during old-receipt checking because compressed arrays were being decompressed at every access. Actual KeyboardInterrupt traceback, source snapshot, chunk, and failure report are retained.
- The repair materializes compressed receipt arrays once. Policy methods, cache, order law, numerical formulas, roster, and thresholds are unchanged.
- `smoke_v2`: all 480 streams and 75,360 decisions completed; original main natural270 + controlled150 and mechanism credit90+90 matched in actions, output labels, correctness, versions, and release order. Numeric scores use the frozen 1e-10 absolute and relative tolerance. Main final hashes and permutation final hashes match exactly. The declared old main tail-flush timestamp exception is counted explicitly in `original_reproduction.json`.

## Resources

Smoke replay plus analysis: 3.794929 s. Old-receipt verification is separately 2.269854 s. Peak RSS: 327467008 bytes. Compressed order chunk: 25433975 bytes. Applying the frozen 128 × 1.25 projection gives 607.188633 s and 4069436000 bytes, within the one-hour / 10-GiB / 4-GiB limits.

## Evidence and remaining gate

`smoke_v2/schema.json` specifies binary arrays and lossless state encoding; `--mode decode --run <run> --order 0 --stream <index>` reconstructs chronological JSONL records. Each run stores immutable source/card/input/config evidence. The full run must use all 128 saved random orders and cannot proceed until a GO JSON binds the verified smoke's source, config, card, orders, and independent-verification hashes.

The independent auditor owns its separate evidence. Full execution has not begun at this report's creation. Resource acceptance cannot select methods or order count based on observed effects.

## Independent smoke closure

The independent checker passed all 480 streams and complete numerical/RNG state. Its report is `independent_smoke_v1/verification.json` (SHA-256 `6dc46467de61b013823b1234278efe585849b01aae78c95dfa3ba3dd0946dbcc`). New-replay score/state maximum absolute error was zero; historical score error was at most 2.22e-16. Coordinator resource GO remains the required next step.
