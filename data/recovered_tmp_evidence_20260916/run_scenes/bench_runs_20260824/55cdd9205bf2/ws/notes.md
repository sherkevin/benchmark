# Notes

## Task Overview
- Task 1: Predict QED for m05 (not synthesized, must use congeneric series trend)
- Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, not in same series as m05

## Pool Structure
Listed all 33 molecules (pages 1-9, budget used: 9).

Grouped by scaffold (free operation):
1. **Scaffold A (COc1ccc(cc1)C(=O)N)**: m15, m24, m28, m17, m22, m05 — m05 is here with chain_len=11, psa=38.33
2. **Scaffold B (O=[N+]([O-])c1ccc(cc1)C(=O)N)**: m09, m02, m12, m32, m33, m29, m31, m06, m10 — psa=72.24
3. **Scaffold C (OCC(O)C(O)C(O)C(=O)N)**: m14, m27, m01, m19, m03, m21, m18, m07, m25 — psa=110.02
4. **Scaffold D (c1ccc(cc1)-c1ccc(cc1)C(=O)N)**: m13, m11, m04, m30, m23, m20, m26, m08, m16 — psa=29.1

## Task 1: Predicting m05 QED
Measured all available molecules in Scaffold A series (m05's congeneric series):
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Used fit_trend with degree=2 polynomial to predict QED at chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Finding a qualifying molecule
Constraints:
1. QED ≥ 0.6
2. psa ≥ 40
3. Not in same series as m05 (not Scaffold A)

Scaffold A has psa=38.33 (< 40), so none qualify anyway.
Scaffold D has psa=29.1 (< 40), so none qualify.
Scaffold B (psa=72.24) and Scaffold C (psa=110.02) are candidates.

Measured molecules to find QED ≥ 0.6:
- m02 (Scaffold B, chain_len=3): QED = 0.6046 ✓ — **QUALIFIES**
- m12 (Scaffold B, chain_len=4): QED = 0.4709 ✗
- m01 (Scaffold C, chain_len=4): QED = 0.3187 ✗

**Selected: m02**
- QED measured: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's scaffold ✓)

## Budget Summary
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 units used
