# Notes for Medicinal Chemistry Task

## Initial Analysis

The pool contains 33 molecules grouped into 4 scaffold groups:

1. Scaffold: `COc1ccc(cc1)C(=O)N` (6 members)
   - m15 (chain_len: 2)
   - m24 (chain_len: 3)
   - m28 (chain_len: 4)
   - m17 (chain_len: 5)
   - m22 (chain_len: 6)
   - m05 (chain_len: 11) - TARGET molecule

2. Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` (9 members)
   - m09 (chain_len: 2)
   - m02 (chain_len: 3)
   - m12 (chain_len: 4)
   - m32 (chain_len: 5)
   - m33 (chain_len: 6)
   - m29 (chain_len: 7)
   - m31 (chain_len: 8)
   - m06 (chain_len: 9)
   - m10 (chain_len: 10)

3. Scaffold: `OCC(O)C(O)C(O)C(=O)N` (9 members)
   - m14 (chain_len: 2)
   - m27 (chain_len: 3)
   - m01 (chain_len: 4)
   - m19 (chain_len: 5)
   - m03 (chain_len: 6)
   - m21 (chain_len: 7)
   - m18 (chain_len: 8)
   - m07 (chain_len: 9)
   - m25 (chain_len: 10)

4. Scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (9 members)
   - m13 (chain_len: 2)
   - m11 (chain_len: 3)
   - m04 (chain_len: 4)
   - m30 (chain_len: 5)
   - m23 (chain_len: 6)
   - m20 (chain_len: 7)
   - m26 (chain_len: 8)
   - m08 (chain_len: 9)
   - m16 (chain_len: 10)

## Task 1 Strategy

For m05 (chain_len: 11), I need to predict its QED by:
1. Measuring QED for some molecules in its congeneric series (same scaffold)
2. Using fit_trend to predict m05's QED based on chain length vs QED relationship

I should measure enough points to establish a reliable trend. The series has 5 measurable members (m15, m24, m28, m17, m22).

## Task 2 Strategy

Need to find a molecule that:
1. QED ≥ 0.6 (must measure to confirm)
2. psa ≥ 40 (from descriptors)
3. Not in same series as m05 (so from scaffolds 2, 3, or 4)

## Budget

Total budget: 17 units (shared between list_pool and measure_qed)
- Page 1 used: 1 unit
- Remaining: 16 units

## Initial Observations from Page 1

Page 1 shows m01-m04:
- m01: psa=110.02 (high) but in scaffold group 3
- m02: psa=72.24 (high) in scaffold group 2  
- m03: psa=110.02 (high) in scaffold group 3
- m04: psa=29.1 (low) in scaffold group 4

From just page 1, m01, m02, m03 have psa ≥ 40 and are not in m05's series.

## Updated Analysis After Viewing All Pages

Budget used: 9 units (pages 1-9)
Budget remaining: 8 units

### Molecules with psa ≥ 40 (potential Task 2 candidates):

**Scaffold Group 2** (`O=[N+]([O-])c1ccc(cc1)C(=O)N`):
- m09 (chain_len: 2, psa: 72.24)
- m02 (chain_len: 3, psa: 72.24)
- m12 (chain_len: 4, psa: 72.24)
- m32 (chain_len: 5, psa: 72.24)
- m33 (chain_len: 6, psa: 72.24)
- m29 (chain_len: 7, psa: 72.24)
- m31 (chain_len: 8, psa: 72.24)
- m06 (chain_len: 9, psa: 72.24)
- m10 (chain_len: 10, psa: 72.24)

**Scaffold Group 3** (`OCC(O)C(O)C(O)C(=O)N`):
- m14 (chain_len: 2, psa: 110.02)
- m27 (chain_len: 3, psa: 110.02)
- m01 (chain_len: 4, psa: 110.02)
- m19 (chain_len: 5, psa: 110.02)
- m03 (chain_len: 6, psa: 110.02)
- m21 (chain_len: 7, psa: 110.02)
- m18 (chain_len: 8, psa: 110.02)
- m07 (chain_len: 9, psa: 110.02)
- m25 (chain_len: 10, psa: 110.02)

**Scaffold Group184** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N`):
- All have psa=29.1 (<40) - NOT eligible

**Scaffold Group 1** (`COc1ccc(cc1)C(=O)N` - m05's series):
- All have psa=38.33 (<40) - NOT eligible for Task 2

So for Task 2, I need to find a molecule from Scaffold Groups 2 or 3 (not Group 1) with QED ≥ 0.6.

## Revised Plan

1. **Task 1**: Measure QED for 3-4 molecules from m05's series (Group 1) to establish trend
2. **Task 2**: Measure QED for promising candidates from Groups 2 or 3 until I find one with QED ≥ 0.6

## Task 1 Measurements Complete

I have measured all 5 measurable molecules in m05's series:

1. m15 (chain_len: 2, QED: 0.7621)
2. m24 (chain_len: 3, QED: 0.7919)
3. m28 (chain_len: 4, QED: 0.7518)
4. m17 (chain_len: 5, QED: 0.7498)
5. m22 (chain_len: 6, QED: 0.7377)

Budget used so far: 14 units (9 for listing + 5 for measurements)
Budget remaining: 3 units

## Task 1 Prediction

Using fit_trend with degree=1 (linear fit) on chain_len vs QED data:
- Points: [(2,0.7621), (3,0.7919), (4,0.7518), (5,0.7498), (6,0.7377)]
- Prediction for chain_len=11 (m05): **0.695**

The quadratic fit (degree=2) predicts 0.5416, but linear seems more reasonable given the gradual decline trend. I'll use 0.695 as my prediction for m05's QED.

## Task 2 Search Results

Budget used: All 17 units used

Measurements taken:
1. m02: QED = 0.6046 ✓ (psa=72.24 ≥ 40, Scaffold Group 2 - not m05's series)
2. m01: QED = 0.3187 ✗ (below 0.6)
3. m03: QED = 0.3282 ✗ (below 0.6)

**Best candidate for Task 2: m02** satisfies all constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold Group 2 (not same as m05's Scaffold Group 1) ✓

## Final Results

Task 1: Predicted QED for m05 (chain_len=11) = **0.695**

Task 2: Selected molecule = **m02** with measured QED = **0.6046**