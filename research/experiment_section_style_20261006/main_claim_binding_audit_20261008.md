# Main-claim binding audit — 2026-10-08

## Purpose

Check that the promoted AAMAS master states only what the current benchmark, baseline matrix, and receipt evidence support. This is a claim audit, not a new experiment and not a replacement for the three acceptance documents.

## Binding table

| Paper location | Claim actually stated | Evidence bound | Audit |
|---|---|---|---|
| Abstract / Introduction | MIRA-Relay updates a provider-local residual from selected, released feedback under an explicit clock | Method equations, selected-only contract, v2 natural/stress receipts | Pass for the declared one-call scope |
| Abstract / Results | Natural fresh panel matches Frozen; declared evaluator-side correctness drift gives the reported gain | 270 natural streams, 150 controlled streams, scaffold intervals, matched controls | Pass; no natural-gain claim |
| Mechanism attribution | Source-aligned selected-provider credit explains the controlled replay relative to fixed cyclic wrong-credit control | `fresh_mechanism_ablation_v2` matched control and report | Pass for evaluator-side drift replay only |
| Validity / Artifact scope | Receipts make read-set, lifecycle, state, and commitment checks reproducible | H7 closure v1: 90/90 streams per condition, 42,480 events, checkpoint/reorder exact, 14/14 mutations, provenance reconstruction | Pass for declared policy replay |
| Scope paragraph / Conclusion | Results do not establish natural lifecycle drift, family sharing, verification/refusal utility, hERG/AMES generalization, or end-to-end agent gain | Explicit exclusions and separate extension contracts | Pass; exclusions are repeated in Results and Conclusion |

## Acceptance status

- **Benchmark + Baseline:** Gate A 15/16, Gate B 13/16, Gate C 14/16; declared-scope pass.
- **Method evidence:** approximately 52/60 on the narrow declared replay; H7=`PASS_FOR_DECLARED_POLICY_REPLAY`; broader method gate remains `HOLD`.
- **Paper layout:** submission build has 9 total pages, 8 body pages, references begin on page 9; warning/fatal/undefined-reference gate passed.
- **Master binding:** `paper/aamas2027/main_provenance.json` points to `build/submission_20261008_044332/main.pdf` with PDF SHA256 `f6c0b6ca473d9b6a65d8ef3e24fad12ce32b5aa6fc0a9beee9e9d19b308cd3d0`.

## Remaining blockers before the broader scientific gate

1. Natural provider/version lifecycle drift is not measured.
2. Family-shared versus provider-local credit is not identified on the fresh four-provider panel.
3. Verification/refusal and end-to-end agent utility are extension contracts, not current results.
4. hERG/AMES and a model-fresh 33-provider confirmation remain outside the current declared scope.

These are scope gaps, not reasons to rewrite the current controlled result. Any future experiment must either close one listed gap or be rejected as unrelated to the paper gate.
