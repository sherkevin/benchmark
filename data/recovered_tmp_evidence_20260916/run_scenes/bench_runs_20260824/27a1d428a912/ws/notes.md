# Notes

## Task 1: QED of m05

m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so direct measurement returns `probe_unavailable`. Must predict from its congeneric series.

### m05's Congeneric Series
Scaffold: `COc1ccc(cc1)C(=O)N`
Members with chain lengths:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6
- m05: chain_len=11 (target)

### Measurements Made
I measured the following molecules in m05's series:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518

Budget was exhausted after m28 (12/12 units used).

### Extrapolation
Using linear fit (degree=1) on points (2, 0.7621), (3, 0.7919), (4, 0.7518):
- Prediction for chain_len=11: **0.7274**

Quadratic fit gave nonsensical negative value (-1.4861) due to extrapolation artifacts.

## Task 2: Find molecule with QED ≥ 0.6, psa ≥ 40, not in m05's series

### Problem
I exhausted the measurement budget on m05's series members. The molecules I measured (m15, m24, m28) all:
1. Have psa=38.33 (< 40, fails constraint 2)
2. Are in the same scaffold as m05 (fails constraint 3)

### Potential Candidates (not measured)
Molecules with psa ≥ 40 and NOT in m05's scaffold:

**Nitro scaffold** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa=72.24):
- m02, m06, m09, m10, m12, m29, m31, m32, m33

**Sugar scaffold** `OCC(O)C(O)C(O)C(=O)N` (psa=110.02):
- m01, m03, m07, m14, m18, m19, m21, m25, m27

None of these were measured due to budget exhaustion.

### Strategic Error
I should have:
1. First identified Task 2 candidates (psa ≥ 40, different scaffold)
2. Measured 1-2 of them to find a QED ≥ 0.6 candidate
3. Used remaining budget for m05 series extrapolation

Instead, I spent all budget on m05's series, leaving no capacity to verify Task 2 candidates.

## Current Status
- Task 1: Predicted QED for m05 = 0.7274 (from linear extrapolation)
- Task 2: Cannot complete - no measured molecules satisfy all three constraints

## Lessons Learned
1. `list_pool` costs 1 unit per page - I wasted 9 units listing all pages
2. `group_by_scaffold` is FREE - should have used it first to identify scaffolds
3. Should have measured only 2 molecules from m05's series (enough for linear fit)
4. Should have reserved budget for measuring Task 2 candidates from other scaffolds

Optimal strategy would have been:
1. Call group_by_scaffold (free) to identify scaffolds
2. List only 1-2 pages of pool (1-2 units) to see descriptors
3. Identify candidates with psa ≥ 40 from non-m05 scaffolds
4. Measure 2 molecules from m05's series for extrapolation (~2 units)
5. Measure 3-4 candidates from other scaffolds to find QED ≥ 0.6 (~3-4 units)
Total: ~6-8 units, leaving buffer
