# Task Analysis

## Understanding the Problem

We have a compound collection of 33 molecules. We need to:
1. Report QED of m05 (which has never been synthesized, so we must predict it from congeneric series)
2. Pick one molecule that satisfies all three constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in same congeneric series as m05

We have a shared budget of 17 units for both `list_pool` (1 unit per page) and `measure_qed` (1 unit per successful measurement).

## Strategy

1. First, use `group_by_scaffold()` to understand the pool structure and identify which molecules are in the same series as m05
2. Use `list_pool()` strategically to find molecules with psa ≥ 40
3. Measure QED for promising candidates while respecting budget constraints
4. Predict m05's QED using `fit_trend()` with measurements from its congeneric series

Let's begin.

## Initial Analysis from group_by_scaffold()

The pool has 4 scaffolds with 33 total molecules:

1. Scaffold 1: "COc1ccc(cc1)C(=O)N" (m05's series)
   - m15 (chain_len=2)
   - m24 (chain_len=3)
   - m28 (chain_len=4)
   - m17 (chain_len=5)
   - m22 (chain_len=6)
   - m05 (chain_len=11) - TARGET

2. Scaffold 2: "O=[N+]([O-])c1ccc(cc1)C(=O)N"
   - m09 (2), m02 (3), m12 (4), m32 (5), m33 (6), m29 (7), m31 (8), m06 (9), m10 (10)

3. Scaffold 3: "OCC(O)C(O)C(O)C(=O)N"
   - m14 (2), m27 (3), m01 (4), m19 (5), m03 (6), m21 (7), m18 (8), m07 (9), m25 (10)

4. Scaffold 4: "c1ccc(cc1)-c1ccc(cc1)C(=O)N"
   - m13 (2), m11 (3), m04 (4), m30 (5), m23 (6), m20 (7), m26 (8), m08 (9), m16 (10)

For Task 2, we need a molecule NOT in Scaffold 1 (m05's series).

## Pool Exploration Results (Budget used: 9 units so far)

Molecules with psa ≥ 40 (candidates for Task 2):

### Scaffold 2 (NOT m05's series):
- m02: psa=72.24
- m06: psa=72.24
- m09: psa=72.24
- m10: psa=72.24
- m12: psa=72.24
- m29: psa=72.24
- m31: psa=72.24
- m32: psa=72.24
- m33: psa=72.24

### Scaffold 3 (NOT m05's series):
- m01: psa=110.02
- m03: psa=110.02
- m07: psa=110.02
- m14: psa=110.02
- m18: psa=110.02
- m19: psa=110.02
- m21: psa=110.02
- m25: psa=110.02
- m27: psa=110.02

### Scaffold 4 (NOT m05's series):
- NONE - all have psa=29.1 (< 40)

Total candidates for Task 2: 18 molecules (9 from scaffold 2, 9 from scaffold 3).

## Task 1: m05 QED Prediction Data

Scaffold 1 measurements (m05's series, chain_len vs QED):
- m15: chain_len=2, QED=0.7621
- m24: chain_len=3, QED=0.7919
- m28: chain_len=4, QED=0.7518
- m17: chain_len=5, QED=0.7498
- m22: chain_len=6, QED=0.7377

m05 chain_len=11 (need to predict).

Using fit_trend() with all 5 points (degree=2): predicted QED = 0.5416
Using fit_trend() with all 5 points (degree=1): predicted QED = 0.695
Using fit_trend() with points 4,5,6 (degree=1): predicted QED = 0.7041
Using fit_trend() with points 4,5,6 (degree=2): predicted QED = 0.5257

Selected prediction: 0.5416 (quadratic fit with all 5 points, RMSE=0.01188)

## Task 2: Candidate Found

m02 satisfies all three constraints:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in same series as m05 (scaffold 2 vs scaffold 1) ✓

Budget used: 9 (list_pool) + 6 (measure_qed: m15,m24,m28,m17,m22,m02) = 15 units.
Remaining budget: 2 units.

## Final Answer

Task 1: Predicted m05 QED = 0.5416 using quadratic fit (degree=2) with all 5 available measurements from its congeneric series.

Task 2: Selected m02 with measured QED = 0.6046, which satisfies all constraints:
- QED ≥ 0.6 ✓ (0.6046)
- psa ≥ 40 ✓ (72.24)
- Not in same series as m05 ✓ (scaffold 2 vs scaffold 1)

Answer saved to answer.json.