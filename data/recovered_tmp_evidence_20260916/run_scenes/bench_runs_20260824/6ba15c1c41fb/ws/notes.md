# Notes for Medicinal Chemistry Task

## Overview
I need to:
1. Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - this molecule has never been synthesized
2. Find a molecule from the pool that satisfies all three constraints:
   - Measured QED ≥ 0.6
   - PSA ≥ 40
   - Not in the same congeneric series as m05 (different scaffold or not just N-alkyl chain length difference)

Budget: 17 units total for both list_pool and measure_qed operations.

## Plan
1. Explore the pool using list_pool to understand the molecules
2. Identify molecules with PSA ≥ 40
3. Measure QED for promising candidates
4. Check which molecules are in the same series as m05
5. Predict QED for m05 based on its congeneric series

## Initial Observations
The task mentions "congeneric series" - molecules with same scaffold, differing only in N-alkyl chain length. m05 has a long N-alkyl chain (CCCCCCCCCCC = 11 carbons).

## Pool Analysis (Budget used: 9/17)
I've reviewed all 33 molecules in the pool. Key observations:

### Molecules with PSA ≥ 40:
1. m01: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCC)
2. m02: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCC)
3. m03: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCCCC)
4. m06: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCC)
5. m07: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCCCCCCC)
6. m09: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCC)
7. m10: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCCC)
8. m12: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC)
9. m14: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCC)
10. m18: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCCCCCC)
11. m19: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCCC)
12. m21: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCCCCC)
13. m25: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCCCCCCCCC)
14. m27: PSA = 110.02 (OCC(O)C(O)C(O)C(=O)NCCC)
15. m29: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCC)
16. m31: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCC)
17. m32: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCC)
18. m33: PSA = 72.24 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCC)

### Molecules with PSA < 40:
All others have PSA = 38.33 or 29.1

### Congeneric Series Analysis:
m05 scaffold: `COc1ccc(cc1)C(=O)N-[alkyl]` (methoxy benzene with amide)
Similar molecules with same scaffold:
- m15: `COc1ccc(cc1)C(=O)NCC` (alkyl = CC, 2 carbons)
- m24: `COc1ccc(cc1)C(=O)NCCC` (alkyl = CCC, 3 carbons)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (alkyl = CCCC, 4 carbons)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (alkyl = CCCCC, 5 carbons)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (alkyl = CCCCCC, 6 carbons)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (alkyl = CCCCCCCCCCC, 11 carbons)

These are all in the same congeneric series (same scaffold, only N-alkyl chain length differs).

### Other Scaffolds:
1. Nitrobenzene series: `O=[N+]([O-])c1ccc(cc1)C(=O)N-[alkyl]` (m02, m06, m09, m10, m12, m29, m31, m32, m33)
2. Biphenyl series: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-[alkyl]` (m04, m08, m11, m13, m16, m20, m23, m26, m30)
3. Polyol series: `OCC(O)C(O)C(O)C(=O)N-[alkyl]` (m01, m03, m07, m14, m18, m19, m21, m25, m27)

## QED Measurements (Budget used:1416 16/17)
1. m01: QED = 0.3187 (too low)
2. m03: QED = 0.3282 (too low)
3. m07: QED = 0.3233 (too low)
4. m02: QED = 0.6046 (≥ 0.6) ✓
5. m06: QED = 0.4011 (too low)
6. m09: QED = 0.5826 (too low, < 0.6)
7. m10: QED = 0.3724 (too low)

m02 satisfies all criteria:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Different scaffold from m05 (nitrobenzene vs methoxybenzene) ✓

No other nitrobenzene molecules tested have QED ≥ 0.6. m09 is close at 0.5826 but doesn't meet the threshold.

## m05 Series Analysis
Measured m15 (2-carbon alkyl): QED = 0.7621
Other series members (not measured due to budget):
- m24 (3C): not measured
- m28 (4C): not measured
- m17 (5C): not measured
- m22 (6C): not measured
- m05 (11C): needs prediction

## QED Prediction for m05
Based on:
1. m15 (2C) has high QED (0.7621)
2. Longer alkyl chains generally decrease QED (observed in nitrobenzene series)
3. m05 has very long alkyl chain (11C), high MW (305.46), high logP (4.956), many rotatable bonds (12)
4. Comparing to nitrobenzene series: 9-10C chains have QED ~0.37-0.40

Prediction: m05 QED ≈ 0.40 (range 0.35-0.45)

## Final Candidate Selection
m02 satisfies all criteria:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Different scaffold from m05 ✓

No budget remaining to test biphenyl series (PSA = 29.1 for all, so don't meet PSA ≥ 40 requirement anyway).

## Final Answer
- m05 predicted QED: 0.50 (based on m15 QED 0.7621 and alkyl chain length trend)
- Lead molecule: m02 with measured QED 0.6046
- Budget used: 17/17 (9 pages + 8 measurements)