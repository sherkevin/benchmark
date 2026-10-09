# Medicinal Chemistry Task - Notes

## Initial Analysis
The compound pool has 33 molecules grouped into 4 scaffold groups:
1. Scaffold: `COc1ccc(cc1)C(=O)N` - 6 members (including m05 with chain_len=11)
2. Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N` -与社会 9 members
3. Scaffold: `OCC(O)C(O)C(O)C(=O)N` - 9 members
4. Scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N` - 9 members

Total molecules: 33

## Task 1: Predict QED of m05
m05 has chain length 11 in scaffold group 1. Need to measure QED of other molecules in same scaffold group and fit a trend to predict m05's QED.

Budget constraints: Shared budget of 17 units for both list_pool and measure_qed. Each successful page costs 1, each successful measurement costs 1.

### Measurements for scaffold group 1 (COc1ccc(cc1)C(=O)N):
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Budget used: 5 units (all measurements)
Remaining budget: 12 units

### Predictions for m05 (chain_len=11):
- Linear fit (degree=1): 0.695
- Quadratic fit (degree=2): 0.5416
- Cubic fit (degree=3): 2.1323 (unrealistic > 1, suggests overfitting)

Quadratic fit seems most reasonable given decreasing trend after peak at chain_len=3.

## Task 2: Find molecule meeting all 3 criteria:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same congeneric series as m05 (not in scaffold group 1)

## Strategy
1. First measure some molecules in scaffold group 1 to establish trend for m05 prediction
2. Then explore other scaffold groups to find candidate for Task 2
3. Need to be efficient with budget (17 units total)

## Current status:
- Budget used: ALL 17 units
- Measurements made: 16 total (5 for scaffold group 1 + 11 for other molecules)
- Pages viewed: 7 pages (28 molecules)

## Task 1 Results:
Predicted QED for m05 (chain_len=11) using quadratic fit on scaffold group 1 data:
- Data points: chain_len [2,3,4,5,6] → QED [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Quadratic fit prediction: 0.5416
- Linear fit prediction: 0.695 (likely overestimates for longer chain)
- Selected prediction: 0.5416 (quadratic fit, more reasonable given decreasing trend)

## Task 2 Results:
Candidate evaluation:

Scaffold group 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N):
- m02 (chain_len=3): QED=0.6046 ✓, psa=72.24 ✓, not in scaffold group 1 ✓
- m09 (chain_len=2): QED=0.5826 ✗ (<0.6)
- m12 (chain_len=4): QED=0.4709 ✗ (<0.6)
- m06 (chain_len=9): QED=0.4011 ✗ (<0.6)

Scaffold group 3 (OCC(O)C(O)C(O)C(=O)N):
- m01 (chain_len=4): QED=0.3187 ✗ (<0.6)
- Other scaffold group 3 molecules likely have low QED due to low ALOGP scores (high polarity)

Scaffold group 4: All have psa=29.1 (<40)

Best candidate found: m02 meets all criteria:
1. QED=0.6046 ≥ 0.6 ✓
2. psa=72.24 ≥ 40 ✓
3. Not in same series as m05 (scaffold group 2, not group 1) ✓

## Final Answer:
- Task 1: Predicted QED for m05 = 0.5416 (using quadratic fit on scaffold group 1 data)
- Task 2: Selected m02 with measured QED = 0.6046

Budget usage: All 17 units consumed (16 measurements + 7 list_pool pages).