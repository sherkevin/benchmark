# Notes for Medicinal Chemistry Task

## Initial Setup
- Task 1: Predict QED for m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - never synthesized
- Task 2: Find a molecule with QED ≥ 0.6, psa ≥ 40, and NOT in same series as m05
- Total budget: 12 units shared between list_pool and measure_qed

## Plan
1. First explore the compound pool structure using group_by_scaffold()
2. Identify which series m05 belongs to and find other molecules in its series
3. Measure QED for several molecules in m05's series to establish trend
4. Use fit_trend to predict m05's QED
5. Search for candidate molecules meeting Task 2 criteria
6. Measure QED of promising candidates

## Results from group_by_scaffold()
4 scaffold groups found:
1. COc1ccc(cc1)C(=O)N (m05's series)
   - Members: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N
   - Members: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. OCC(O)C(O)C(O)C(=O)N
   - Members: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N
   - Members: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

m05 has chain length 11, other members in its series have chain lengths 2-6.

## Next Steps
1. Browse pool to see descriptors (especially psa for Task 2 filtering)
2. Measure QED for some molecules in m05's series
3. Predict m05 QED
4. Find candidate for Task 2

## Pool Browsing (Page 1)
Budget used: on page load: 1, Budget remaining: 12
- m01: psa=110.02 (≥40), scaffold group 3
- m02: psa=72.24 (≥40), scaffold group 2  
- m03: psa=110.02 (≥40), scaffold group 3
- m04: psa=29.1 (<40), scaffold group 4

Already seeing potential Task 2 candidates (m01, m02, m03) with psa≥40.

## QED Measurements for m05 Series Trend
Budget used: 6 total (1 list_pool + 5 measure_qed)
- m15 (chain_len 2): QED = 0.7621
- m24 (chain_len 3): QED = 0.7919
- m28 (chain_len 4): QED = 0.7518
- m17 (chain_len 5): QED = 0.7498
- m22 (chain_len 6): QED = 0.7377

Now have 5 points to predict m05 (chain_len 11).

## Task 1 Prediction
Using quadratic fit (degree=2) with chain lengths [2,3,4,5,6] and QEDs [0.7621,0.7919,0.7518,0.7498,0.7377]:
- Predicted QED for m05 (chain_len 11): 0.5416

This will be our `target_qed` in answer.json.

## Task 2 Candidate Search
Budget used: 10 total (5 list_pool + 5 measure_qed), Remaining: 2

Candidates with psa≥40 (not in m05's series):
Scaffold Group 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N):
- m02: psa=72.24, chain_len 3
- m06: psa=72.24, chain_len 9  
- m09: psa=72.24, chain_len 2
- m10: psa=72.24, chain_len 10
- m12: psa=72.24, chain_len 4

Scaffold Group 3 (OCC(O)C(O)C(O)C(=O)N):
- m01: psa=110.02, chain_len 4
- m03: psa=110.02, chain_len 6
- m07: psa=110.02, chain_len 9
- m14: psa=110.02, chain_len 2
- m18: psa=110.02, chain_len 8
- m19: psa=110.02, chain_len 5

Need to measure QED of 1-2 promising candidates with budget 2 left.

## Task 2 Solution Found
Measured m02 (scaffold group 2, psa=72.24):
- QED = 0.6046 (≥0.6 ✓)
- psa = 72.24 (≥40 ✓)  
- Not in same series as m05 (different scaffold ✓)

m02 satisfies all three Task 2 constraints!

## Final Budget Usage
Total budget: 12 units
Used: 11 units (5 list_pool + 6 measure_qed)
Remaining: 1 unit

## Final Answer
- target_qed: 0.5416 (predicted QED for m05)
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046

Both tasks completed successfully.