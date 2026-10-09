# Notes

## Task 1: Report QED of m05

m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11.

Measured QED for molecules in the same congeneric series:
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

Fit a quadratic trend through these points and evaluated at x=11:
- Prediction for m05: QED = 0.5416

## Task 2: Pick one molecule with QED ≥ 0.6, psa ≥ 40, not in same series as m05

Candidates must have psa ≥ 40 and NOT be from scaffold `COc1ccc(cc1)C(=O)N`.

Measured:
- m01: QED=0.3187 (psa=110.02) - QED < 0.6, fails
- m02: QED=0.6046 (psa=72.24) - QED ≥ 0.6 ✓, psa ≥ 40 ✓, scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from m05) ✓
- m14: QED=0.3237 (psa=110.02) - QED < 0.6, fails
- m09: QED=0.5826 (psa=72.24) - QED < 0.6, fails

**Selected molecule: m02**
- QED measured: 0.6046
- psa: 72.24 (≥ 40)
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (different from m05's scaffold)

## Budget used
9 measurements out of 12.
