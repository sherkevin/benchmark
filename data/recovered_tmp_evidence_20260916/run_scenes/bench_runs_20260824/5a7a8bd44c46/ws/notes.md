# Notes

## Task Overview
- Task 1: Report QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - it's unsynthesized, so must predict from congeneric series
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, NOT in same congeneric series as m05

## Scaffolds identified (4 groups)
1. **COc1ccc(cc1)C(=O)N** - m05's scaffold (m15, m24, m28, m17, m22, m05) - chain lengths 2,3,4,5,6,11
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N** - nitro group (m09, m02, m12, m32, m33, m29, m31, m06, m10) - chain lengths 2-10
3. **OCC(O)C(O)C(O)C(=O)N** - sugar-like (m14, m27, m01, m19, m03, m21, m18, m07, m25) - chain lengths 2-10
4. **c1ccc(cc1)-c1ccc(cc1)C(=O)N** - biphenyl (m13, m11, m04, m30, m23, m20, m26, m08, m16) - chain lengths 2-10

## Task 2 constraints
- QED >= 0.6
- psa >= 40
- NOT in scaffold group 1 (m05's series)

So eligible molecules for Task 2 must be from scaffolds 2, 3, or 4.

Looking at psa values from catalog:
- Scaffold 1 (m05's series): psa = 38.33 (all members) - this is < 40, so none would qualify anyway
- Scaffold 2 (nitro): psa = 72.24 (all members) - qualifies on psa
- Scaffold 3 (sugar): psa = 110.02 (all members) - qualifies on psa
- Scaffold 4 (biphenyl): psa = 29.1 (all members) - does NOT qualify (psa < 40)

So for Task 2, candidates are only from scaffold 2 (nitro) or scaffold 3 (sugar).

## Strategy
1. For Task 1: Measure QED of several molecules in m05's scaffold (group 1) to build a trend, then predict m05's QED
2. For Task 2: Measure QED of molecules from scaffold 2 or 3 to find one with QED >= 0.6

Budget: 12 total, used 9 for listing pool. Remaining: 3 units.

## Measurements made
- m15 (chain 2, scaffold 1): QED = 0.7621
- m24 (chain 3, scaffold 1): QED = 0.7919
- m28 (chain 4, scaffold 1): QED = 0.7518

Budget exhausted (12/12).

## Problem
All measured molecules are in m05's scaffold (scaffold 1), so none can be used for Task 2.
I need a molecule from scaffold 2 or 3 (psa >= 40) with measured QED >= 0.6.
But I have no budget left to measure such molecules.

## Prediction for m05
Using linear fit (degree 1) on chain lengths 2, 3, 4 with QEDs 0.7621, 0.7919, 0.7518:
- Predicted QED for chain length 11: 0.7274

Using quadratic fit (degree 2):
- Predicted QED for chain length 11: -1.4861 (invalid, negative)

Linear fit seems more reasonable for extrapolation.

## Updated analysis
Looking at the trend:
- Chain 2: 0.7621
- Chain 3: 0.7919 (peak)
- Chain 4: 0.7518 (decreasing)

The trend from 3 to 4 shows decreasing QED. Extrapolating to chain 11:
- Using linear fit on points 3,4: prediction = 0.4711

This suggests m05's QED is around 0.47.

## Task 2 problem
I have no budget left to measure molecules from scaffold 2 or 3.
All my measured molecules (m15, m24, m28) are in scaffold 1 (m05's series).
Cannot complete Task 2 as specified without measuring a valid candidate.

Possible candidates (unmeasured) from scaffold 2 or 3:
- Scaffold 2 (nitro, psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Scaffold 3 (sugar, psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

Without measurements, I cannot know which have QED >= 0.6.

## Final decision
Due to budget exhaustion, I cannot complete Task 2 as specified.
- I measured only molecules from scaffold 1 (m05's series): m15, m24, m28
- None of these can be used for Task 2 (constraint 3)
- I have no budget left to measure molecules from scaffold 2 or 3

For Task 1:
- Predicted QED for m05 (chain 11): 0.47 (based on linear extrapolation from chain 3-4 trend)

For Task 2:
- Cannot provide a valid molecule with measured QED >= 0.6 from scaffold 2 or 3
- Best guess: m02 (scaffold 2, psa=72.24) might have QED >= 0.6, but unmeasured

## Lesson learned
Should have measured more strategically:
- 2 molecules from m05's series for trend
- 1 molecule from scaffold 2 or 3 for Task 2

## Final Answer
- target_qed: 0.47 (predicted for m05 based on linear extrapolation from chain 3-4 trend)
- lead_mol_id: null (cannot provide - no budget left to measure candidates from scaffold 2 or 3)
- lead_qed_measured: null (cannot provide - no measurements of valid candidates)

Task 2 could not be completed due to budget exhaustion from listing all 9 pages and measuring only molecules from m05's series.
