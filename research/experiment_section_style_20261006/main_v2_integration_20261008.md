# Main-text v2 mechanism integration record

Date: 2026-10-08 (Asia/Shanghai)

## Purpose

Bind the fresh mechanism-ablation v2 evidence to the paper-level master without
mixing its scaffold-mean estimand with the existing pooled confirmation tables.
The previous `main.pdf` is preserved by the promotion tool.

## Source and build

- Source: `paper/aamas2027/main.tex`
- Source SHA-256: `4272e3689ddd3c88be317c948b0608bae30b0d32c3d65e1be4a5ec4d1c7ac045`
- Preflight build: `paper/aamas2027/build/submission_20261007_161243/main.pdf`
- PDF SHA-256: `788488c2c3d02e613f38cca0b91af6c2cbb7c10cbd19724e6b0ebe346669269a`
- Log SHA-256: `ba0bb06b4b9eeeb8c0994972ca180d36f570ef0542367eef9efb0ad95739067d`
- Page gate: 9 total pages; body pages 1--8; References starts on page 9.
- Warning gate: no `Overfull`, `Undefined`, `Citation`, missing-math, `Fatal`, or
  `Emergency` markers in the final log.
- Promotion: `paper/aamas2027/main.pdf` now matches the preflight PDF byte for byte;
  prior master and provenance are in `paper/aamas2027/archive/promotions/`.

## Evidence binding

The main text adds a short mechanism-attribution paragraph and a mechanism-control
role in the baseline table. The existing pooled natural/stress tables are unchanged.
The new paragraph reports only the v2 matched replay:

- Frozen, MIRA-Relay, and fixed cyclic permutation share cold-start scores/actions;
  only released residual destination changes.
- Equal-weight scaffold-mean estimand; 90 streams per condition; two deterministic
  schedule replicas; delays $D\in\{0,1,4\}$.
- Controlled Local--permutation: `+15.67 pp`, 95% CI `[+10.65,+20.74]`.
- Controlled permutation--Frozen: `-1.27 pp`, 95% CI `[-3.66,+0.95]`.
- Natural Local--permutation: `-0.30 pp`, 95% CI `[-1.42,+0.76]`.

The source paragraph explicitly limits the interpretation to source-aligned credit
under the declared evaluator-side drift replay. It does not claim natural provider
drift, family sharing, verification/refusal utility, or end-to-end agent gain.

## Visual check

Rendered pages 8 and 9 at 140 dpi and inspected them. Page 8 contains the complete
Conclusion; page 9 contains the references only. No clipping or table overflow was
observed. The remaining whitespace on the final body page is recorded as a layout
quality issue for a later writing pass, not as a page-count failure.

## Gate status

This integration closes the paper-text binding for the v2 mechanism evidence. It
does not change the independent method score or open the full scientific gate:
declared-scope Benchmark/Baseline gates remain passable, while the method gate is
still HOLD because H7 is replay-scope partial and the paper does not establish
natural provider drift, provider inference/offline-fit reconstruction, or the
broader verifier/refusal/agent claims.
