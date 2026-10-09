# Notes

## Task Overview
- Task 1: Report QED of m05 (which has never been synthesized, so must predict from congeneric series)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, and NOT in same congeneric series as m05

## Understanding m05's Congeneric Series
m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (SMILES with N-alkyl chain)

m05's scaffold: `COc1ccc(cc1)C(=O)N`
Members of m05's series: m15 (len=2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)

## Task 1: Predicting m05's QED

Measured QED for m05's congeneric series:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Used fit_trend with degree=2 polynomial to predict QED at chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Finding a Lead Molecule

Criteria:
1. QED >= 0.6
2. psa >= 40
3. NOT in m05's congeneric series

Molecules with psa >= 40 NOT in m05's series:
- O=[N+]([O-])c1ccc(cc1)C(=O)N series: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
- OCC(O)C(O)C(O)C(=O)N series: m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)

Measured QED for candidates:
- m01: QED = 0.3187 (fails QED >= 0.6)
- m02: QED = 0.6046 ✓ (passes all criteria!)
- m09: QED = 0.5826 (fails QED >= 0.6)
- m14: QED = 0.3237 (fails QED >= 0.6)

**Selected lead molecule: m02**
- QED: 0.6046 (>= 0.6) ✓
- psa: 72.24 (>= 40) ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's scaffold) ✓

## Budget Used
- 5 measurements for m05's series (m15, m24, m28, m17, m22)
- 4 measurements for Task 2 candidates (m01, m02, m09, m14)
- Total: 9 of 12 budget used
