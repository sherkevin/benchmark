# Notes for QED Measurement Task

## Task Overview
- Need to determine QED for m05 (Task 1)
- Need to find a molecule that meets all three criteria (Task 2):
  1. Measured QED ≥ 0.6
  2. psa ≥ 40
  3. Not in same congeneric series as m05
- Budget: 12 measurements total

## Initial Analysis
- Molecule m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (has methoxy group, carbonyl, and long alkyl chain)
- Molecules in same congeneric series as m05 would have the same scaffold (methoxyphenyl-carboxamide) with different N-alkyl chain lengths
- From visual inspection, molecules with similar scaffold to m05: m15, m17, m22, m24, m28 (all have COc1ccc(cc1)C(=O)N- pattern with different alkyl chain lengths)

## Plan
1. Try measuring m05 first to confirm it returns probe_unavailable
2. Identify molecules that are NOT in the same congeneric series as m05
3. Among those, identify ones with psa ≥ 40
4. Measure QED for promising candidates until finding one with QED ≥ 0.6

## Measurements
1. m05 -> probe_unavailable (as expected)
2. m01 -> QED = 0.3187 (< 0.6, rejected)
3. m02 -> QED = 0.6046 (≥ 0.6, meets all criteria!)
4. m03 -> QED = 0.3282 (< 0.6, rejected)
5. m06 -> QED = 0.4011 (< 0.6, rejected)
6. m07 -> QED = 0.3233 (< 0.6, rejected)
7. m09 -> QED = 0.5826 (< 0.6, rejected)
8. m10 -> QED = 0.3724 (< 0.6, rejected)
9. m12 -> QED = 0.4709 (< 0.6, rejected)
10. m14 -> QED = 0.3237 (< 0.6, rejected)
11. m29 -> QED = 0.4446 (< 0.6, rejected)
12. m31 -> QED = 0.4252 (< 0.6, rejected)
13. m32 -> QED = 0.4683 (< 0.6, rejected)

## Results
- Task 1: m05 cannot be measured directly. I should have measured some compounds from the same congeneric series (m15, m24, m28, m17, m22) to establish a trend for predicting m05's QED. Since I didn't do this, I cannot properly predict based on the series trend as requested. However, based on general chemical principles, longer alkyl chains typically reduce drug-likeness, so m05 with its 11-carbon chain likely has a low QED. Without actual measurements from the series, this remains speculative.
- Task 2: m02 is the best candidate with:
  - mol_id: m02
  - SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
  - QED: 0.6046 (≥ 0.6 ✓)
  - psa: 72.24 (≥ 40 ✓)
  - Not in same congeneric series as m05 ✓

## Reflection
I should have prioritized measuring compounds from m05's congeneric series (m15, m24, m28, m17, m22) first to establish a trend for predicting m05's QED, before moving on to other candidates for Task 2. With only 12 measurements, I could have allocated them differently, perhaps measuring 3-4 compounds from m05's series to establish a trend, then using the remaining measurements for Task 2.