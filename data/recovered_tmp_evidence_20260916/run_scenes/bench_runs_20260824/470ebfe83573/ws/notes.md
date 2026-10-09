# Medicinal Chemistry Task Notes

## Overview
I need to complete two tasks:
1. Predict the QED of m05 (which cannot be measured directly)
2. Find a molecule with QED ≥ 0.6, psa ≥ 40, and not in the same series as m05

Budget: frequency limit 12 units total shared between list_pool and measure_qed.

## Scaffold Groups
From `group_by_scaffold()`:
1. Group 1: Scaffold "COc1ccc(cc1)C(=O)N" (m05's series)
   - m15 (chain_len=2)
   - m24 (chain_len=3)
   - m28 (chain_len=4)
   - m17 (chain_len=5)
   - m22 (chain_len=6)
   - m05 (chain_len=11) - target molecule

2. Group 2: Scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N"
   - m09, m02, m12, m32, m33, m29, m31, m06, m10 (chain_len 2-10)

3. Group 3: Scaffold "OCC(O)C(O)C(O)C(=O)N"
   - m14, m27, m01, m19, m03, m21, m18, m07, m25 (chain_len 2-10)

4. Group 4: Scaffold "c1ccc(cc1)-c1ccc(cc1)C(=O)N"
   - m13, m11, m04, m30, m23, m20, m26, m08, m16 (chain_len 2-10)

## Approach
1. First, measure QEDs of some members in m05's series to establish a trend
2. Use `fit_trend()` to predict m05's QED
3. Browse pool to find molecules with psa ≥ 40
4. Measure QEDs of promising candidates
5. Find a molecule meeting all constraints

## QED Measurements in m05's Series
Budget used:等在: 6 (1 list_pool + 5 measure_qed)
Remaining budget: 6

m15 (chain_len=2): QED = 0.7621
m24 (chain_len=3): QED = 0.7919
m28 (chain_len=4): QED = 0.7518
m17 (chain_len=5): QED = 0.7498
m22 (chain_len=6): QED = 0.7377

Chain lengths: [2, 3, 4, 5, 6]
QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
m05 chain length: 11

## Predictions for m05
Using 5 points (chain_len 2-6):
- Quadratic fit (degree=2): QED ≈ 0.5416
- Linear fit (degree=1): QED ≈ 0.6950

The linear fit seems more reasonable given the decreasing trend. Quadratic fit might be overfitting.

## Task 2 Candidates (psa ≥ 40, not in m05's series)
From pages examined:
--> Group 2 (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
--> Group 3 (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

## Measurements Made
1. m15: QED=0.7621 (psa=38.33 < 40, group 1 - m05's series)
2. m24: QED=0.7919 (psa=38.33 < 40, group 1 - m05's series)
3. m28: QED=0.7518 (psa=38.33 < 40, group 1 - m05's series)
4. m17: QED=0.7498 (psa=38.33 < 40, group 1 - m05's series)
5. m22: QED=0.7377 (psa=38.33 < 40, group 1 - m05's series)
6. m06: QED=0.4011 (psa=72.24 ≥ 40, group 2, not m05's series, but QED < 0.6)

## Problem
No molecule measured meets all 3 criteria:
1. QED ≥ 0.6
2. psa ≥ 40
3. Not in m05's series

Budget exhausted: 12 units used (6 list_pool + 6 measure_qed)

## Strategic Errors
1. Measured too many molecules in m05's series (5) when fewer would suffice
2. Only measured one candidate for Task 2 (m06) which had low QED due to nitro alerts
3. Should have measured group 3 molecules instead of group 2 (no nitro alerts)
4. Should have checked psa values before measuring

## Final Decisions
1. For m05 QED prediction: Using linear fit (0.695) as more conservative than quadratic (0.5416)
2. For lead molecule: m06 is the only measured molecule with psa ≥ 40 and not in m05's series, but its QED (0.4011) < 0.6

## Submitted Answer
{
 "target_qed": 0.695,
 "lead_mol_id": "m06",
 "lead_qed_measured": 0.4011
}

Note: m06 does not satisfy QED ≥ 0.6 constraint, but it's the only candidate measured that satisfies psa ≥ 40 and not in m05's series.