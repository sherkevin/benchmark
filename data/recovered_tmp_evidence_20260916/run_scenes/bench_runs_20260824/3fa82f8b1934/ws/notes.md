# Notes

## Task Overview
- Task 1: Report QED of m05 (not synthesized, need to predict from congeneric series)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, NOT in same series as m05

## Scaffold Groups
m05 is in the `COc1ccc(cc1)C(=O)N` scaffold group with chain_len=11:
- m15 (chain_len=2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)

Other scaffolds (NOT same series as m05):
1. `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m09, m02, m12, m32, m33, m29, m31, m06, m10
2. `OCC(O)C(O)C(O)C(=O)N`: m14, m27, m01, m19, m03, m21, m18, m07, m25
3. `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: m13, m11, m04, m30, m23, m20, m26, m08, m16

## Strategy
- For Task 1: Measure QED of several molecules in m05's scaffold series (different chain lengths), fit a trend, and predict m05's QED at chain_len=11
- For Task 2: Need molecules from OTHER scaffolds with psa >= 40

## Molecules with psa >= 40 (from pool.tsv)
Looking at the data:
- m01: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m03: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m07: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m14: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m18: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m19: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m21: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m25: psa=110.02 (scaffold OCC(O)... - NOT m05 series) ✓
- m02: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m06: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m09: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m10: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m12: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m29: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m31: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m32: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓
- m33: psa=72.24 (scaffold O=[N+]... - NOT m05 series) ✓

Molecules in m05's series (COc1ccc...): m15, m24, m28, m17, m22, m05 - all have psa=38.33 (< 40)

So for Task 2, candidates are from the OCC(O)... and O=[N+]... scaffolds.

## Budget: 12 measurements total

### Measurements Made (12 total)
For m05 series (Task 1):
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

Fit trend (degree=2) at x_query=11 → **m05 predicted QED = 0.5416**

For Task 2 candidates:
- m14: QED=0.3237 (psa=110.02) - too low
- m01: QED=0.3187 (psa=110.02) - too low
- m09: QED=0.5826 (psa=72.24) - close but < 0.6
- m02: QED=0.6046 (psa=72.24) - **MEETS ALL CRITERIA!**

### Selected Lead for Task 2: m02
- QED = 0.6046 (>= 0.6) ✓
- psa = 72.24 (>= 40) ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (NOT same as m05's COc1ccc... scaffold) ✓
