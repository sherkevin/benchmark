# Notes

## Task Overview
- Task 1: Report QED of m05 (cannot measure directly, must predict from congeneric series)
- Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, NOT in same congeneric series as m05
- Budget: 12 measurements total

## m05 Congeneric Series
m05 has scaffold `COc1ccc(cc1)C(=O)N-` with varying N-alkyl chain lengths.
Molecules in same series (same scaffold, differ only in chain length):
- m05: C13 chain (target, not synthesizable)
- m15: C2 chain
- m17: C5 chain
- m22: C6 chain
- m24: C3 chain
- m28: C4 chain

## Strategy
1. Measure QED for m05 congeneric series to establish trend and predict m05's QED
2. Measure QED for other molecules with psa ≥ 40 to find a valid lead

## Molecules with psa ≥ 40 (excluding m05 series)
- m01: psa=110.02 (scaffold: OCC(O)C(O)C(O)C(=O)N-)
- m02: psa=72.24 (scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N-)
- m03: psa=110.02
- m06: psa=72.24
- m07: psa=110.02
- m09: psa=72.24
- m10: psa=72.24
- m12: psa=72.24
- m14: psa=110.02
- m18: psa=110.02
- m19: psa=110.02
- m21: psa=110.02
- m25: psa=110.02
- m27: psa=110.02
- m29: psa=72.24
- m31: psa=72.24
- m32: psa=72.24
- m33: psa=72.24

## Measurements Taken (12/12 used)

### m05 Congeneric Series:
- m15 (C2): QED = 0.7621
- m24 (C3): QED = 0.7919
- m28 (C4): QED = 0.7518
- m17 (C5): QED = 0.7498
- m22 (C6): QED = 0.7377

### Task 2 Candidates:
- m01: QED = 0.3187 (psa=110.02) - FAILS QED < 0.6
- m02: QED = 0.6046 (psa=72.24) - PASSES ALL!
- m09: QED = 0.5826 (psa=72.24) - FAILS QED < 0.6
- m12: QED = 0.4709 (psa=72.24) - FAILS QED < 0.6
- m14: QED = 0.3237 (psa=110.02) - FAILS QED < 0.6
- m32: QED = 0.4683 (psa=72.24) - FAILS QED < 0.6
- m33: QED = 0.4591 (psa=72.24) - FAILS QED < 0.6

## Task 1: Predicting m05 QED

Linear regression on congeneric series (chain length vs QED):
- Using all 5 points: QED ≈ 0.795 - 0.0091*chain_length
- For chain_length=13: QED ≈ 0.795 - 0.118 = 0.677

Using declining portion (C3-C6):
- From C3 (0.7919) with slope ~0.018 per carbon
- For C13: 0.7919 - 10*0.018 ≈ 0.61

Considering m05 properties (high alogp=4.956, high rotb=12, mw=305):
The QED is likely on the lower end of the extrapolation.

Prediction: ~0.60-0.65, I'll use 0.62

## Task 2: Selected Molecule

m02 satisfies all criteria:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in m05 series (nitro-phenyl scaffold, not methoxy-phenyl) ✓

**Answer: m02 with measured QED = 0.6046**
