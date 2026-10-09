# Notes on QED Prediction and Lead Identification

## Task 1: Predicting QED for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so direct measurement returns probe_unavailable. However, I identified it belongs to a congeneric series with scaffold COc1ccc(cc1)C(=O)N, which includes molecules m15, m24, m28, m17, m22, and m05 (differing only by N-alkyl chain length).

I measured the QED of 5 molecules in this series:
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

Using these data points, I fitted a polynomial trend and extrapolated to predict the QED for m05 (chain length 11), obtaining a predicted QED of 0.5416.

## Task 2: Finding a suitable lead compound

I needed to find a molecule satisfying all three criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same congeneric series as m05

First, I identified molecules with PSA ≥ 40 from different series than m05. Then I measured their QED values:

From the polyol series (OCC(O)C(O)C(O)C(=O)N):
- m01: QED = 0.3187 (< 0.6, failed)
- m03: QED = 0.3282 (< 0.6, failed)
- m07: QED = 0.3233 (< 0.6, failed)
- m14: QED = 0.3237 (< 0.6, failed)

From the nitrobenzene series (O=[N+]([O-])c1ccc(cc1)C(=O)N):
- m02: QED = 0.6046 (≥ 0.6, passed!)
- m09: QED = 0.5826 (< 0.6, failed)
- m12: QED = 0.4709 (< 0.6, failed)

Molecule m02 has:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Different series from m05 ✓

Therefore, m02 is the ideal candidate for Task 2.

## Summary
- Target QED for m05: 0.5416
- Lead molecule: m02
- Lead QED measured: 0.6046