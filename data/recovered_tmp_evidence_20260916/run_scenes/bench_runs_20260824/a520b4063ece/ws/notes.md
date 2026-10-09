# Task Notes

## Task Overview
- Need to predict QED for m05 (target molecule) which has never been synthesized
- Need to find one molecule from pool with:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. not in same congeneric series as m05 (different scaffold or not just N-alkyl chain length differences)
- Shared budget of 17 units for both list_pool and measure_qed operations

## Initial Strategy
1. First, browse the pool to understand the molecules available
2. Identify molecules that might be in the same congeneric series as m05 to understand the trend
3. Measure QED for several molecules in m05's series to predict m05's QED
4. Then search for candidate molecules meeting the three criteria
5. Measure QED for promising candidates

## Initial Exploration
Budget: 13 remaining (4 used for pages 1-4)

Page 1 molecules:
- m01: SMILES OCC(O)C(O)C(O)C(=O)NCCCC, mw=221.25, alogp=-2.022, psa=110.02, rotb=7, arom=0
- m02: SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCCC, mw=208.22, alogp=1.735, psa=72.24, rotb=4, arom=1
- m03: SMILES OCC(O)C(O)C(O)C(=O)NCCCCCC, mw=249.31, alogp=-1.242, psa=110.02, rotb=9, arom=0
- m04: SMILES c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC, mw=253.34, alogp=3.884, psa=29.1, rotb=5, arom=2

Page 2 molecules:
- m05: SMILES COc1ccc(cc1)C(=O)NCCCCCCCCCCC, mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1 (TARGET)
- m06: SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCC, mw=292.38, alogp=4.075, psa=72.24, rotb=10, arom=1
- m07: SMILES OCC(O)C(O)C(O)C(=O)NCCCCCCCCC, mw=291.39, alogp=-0.072, psa=110.02, rotb=12, arom=0
- m08: SMILES c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCC, mw=323.48, alogp=5.834, psa=29.1, rotb=10, arom=2

Page79
- m09: SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCC, mw=194.19, alogp=1.345, psa=72.24, rotb=3, arom=1
- m10: SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCCC, mw=306.41, alogp=4.465, psa=72.24, rotb=11, arom=1
- m11: SMILES c1ccc(cc1)-c1ccc(cc1)C(=O)NCCC, mw=239.32, alogp=3.493, psa=29.1, rotb=4, arom=2
- m12: SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC, mw=222.24, alogp=2.125, psa=72.24, rotb=5, arom=1

## Complete Pool Scan
Budget: 8 remaining (9 used for pages 1-9)

Found molecules in m05's congeneric series (COc1ccc(cc1)C(=O)N... scaffold):
- m15: COc1ccc(cc1)C(=O)NCC (2 carbons)
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbons)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbons)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbons)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbons)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (12 carbons) - TARGET

Other series observed:
- O=[N+]([O-])c1ccc(cc1)C(=O)N... series (nitrobenzamide): m02, m06, m09, m10, m12, m29, m31, m32, m33
- OCC(O)C(O)C(O)C(=O)N... series (sugar amide): m01, m03, m07, m14, m18, m19, m21, m25, m27
- c1ccc(cc1)-c1ccc(cc1)C(=O)N... series (biphenyl amide): m04, m08, m11, m13, m16, m20, m23, m26, m30

## QED Measurements for m05 Series
Budget: 3 remaining (14 used total: 9 pages + 5 measurements)

Chain length vs QED:
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

Pattern: QED seems to generally decrease with increasing chain length, with m24 (3C) being an outlier high value.

## Final Measurements and Findings
Budget: 0 remaining (17 used total: 9 pages + 8 measurements)

### Task 1: m05 QED Prediction
Based on measured QED values for m05 series:
- 2C (m15): 0.7621
- 3C (m24): 0.7919 (outlier)
- 4C (m28): 0.7518
- 5C (m17): 0.7498
- 6C (m22): 0.7377

Trend shows decreasing QED with increasing alkyl chain length. For 12C (m05), QED should be lower. Extrapolating from 6C value (0.7377) with continued decrease, predicted QED ≈ 0.69.

**Final prediction for m05 QED: 0.69**

### Task 2: Candidate Selection
Tested candidates:
- m02 (nitro series): QED=0.6046, psa=72.24 ≥ 40, not in m05 series ✓ MEETS ALL CRITERIA
- m01 (sugar series): QED=0.3187 (too low)
- m06 (nitro series): QED=0.4011 (too low)

m02 is a valid candidate meeting all three requirements:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓  
3. not in m05 series (nitrobenzamide vs methoxybenzamide) ✓

**Selected molecule: m02 with QED 0.6046**

## Final Summary
- **Target QED (m05)**: Predicted 0.69 based on decreasing trend in congeneric series
- **Lead molecule**: m02 (SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC)
- **Lead QED**: 0.6046 (measured)
- **Budget usage**: 17/17 units (9 pages + 8 measurements)
- **Answer.json updated** with correct values