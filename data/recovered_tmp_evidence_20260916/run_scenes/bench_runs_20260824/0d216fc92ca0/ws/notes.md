# Notes for Molecular Design Task

## Overview
- Working with 33 molecules in data/pool.tsv
- Need to measure QED values with a budget of 12 measurements total
- Two main tasks:
  1. Predict QED for m05 (which cannot be measured directly as it's not synthesized)
  2. Find a molecule meeting three criteria

## Task 1: m05 Analysis
- m05 has SMILES COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- It belongs to the scaffold "COc1ccc(cc1)C(=O)N" with chain length 11
- Other members of this series: m15 (chain 2), m24 (chain 3), m28 (chain 4), m17 (chain 5), m22 (chain 6)
- Since m05 has never been synthesized, measure_qed returns probe_unavailable
- Need to predict based on trend analysis of the congeneric series

## Task 2: Molecule Selection Criteria
1. measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same congeneric series as m05 (not with scaffold COc1ccc(cc1)C(=O)N)

## Measurement Plan
I'll measure QED values for molecules in the same series as m05 to establish a trend, then measure candidates for Task 2.

First, let me measure the QED values for the molecules in the same series as m05:
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377

Now I need to fit a trend to predict the QED for m05 (chain length 11).

For Task 2, I need to find a molecule that:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same congeneric series as m05 (not with scaffold COc1ccc(cc1)C(=O)N)

The other series are:
- "O=[N+]([O-])c1ccc(cc1)C(=O)N" with members: m09, m02, m12, m32, m33, m29, m31, m06, m10
- "OCC(O)C(O)C(O)C(=O)N" with members: m14, m27, m01, m19, m03, m21, m18, m07, m25
- "c1ccc(cc1)-c1ccc(cc1)C(=O)N" with members: m13, m11, m04, m30, m23, m20, m26, m08, m16

From the TSV file, I need to identify molecules with psa ≥ 40:
- m01: psa = 110.02
- m02: psa = 72.24
- m03: psa = 110.02
- m07: psa = 110.02
- m14: psa = 110.02
- m18: psa = 110.02
- m19: psa = 110.02
- m21: psa = 110.02
- m25: psa = 110.02
- m27: psa = 110.02

These are all from series 2 ("OCC(O)C(O)C(O)C(=O)N") and series 4 ("c1ccc(cc1)-c1ccc(cc1)C(=O)N").

Wait, I also see:
- m06: psa = 72.24
- m09: psa = 72.24
- m10: psa = 72.24
- m12: psa = 72.24
- m29: psa = 72.24
- m31: psa = 72.24
- m32: psa = 72.24
- m33: psa = 72.24

These are from series 1 ("O=[N+]([O-])c1ccc(cc1)C(=O)N").

So potential candidates for Task 2 are from series 1, 2, and 4, excluding the series containing m05 (series 3).

Results from measurements:
- m09: QED = 0.5826 (less than 0.6 threshold)
- m02: QED = 0.6046 (meets threshold!)
- m12: QED = 0.4709 (less than 0.6 threshold)
- m32: QED = 0.4683 (less than 0.6 threshold)
- m33: QED = 0.4591 (less than 0.6 threshold)

For Series 2:
- m01: QED = 0.3187 (less than 0.6 threshold)
- m14: QED = 0.3237 (less than 0.6 threshold)

Based on the results, m02 is the only molecule that meets all three criteria:
1. measured QED ≥ 0.6: 0.6046 ≥ 0.6 ✓
2. psa ≥ 40: 72.24 ≥ 40 ✓
3. Not in the same congeneric series as m05: m02 is from series "O=[N+]([O-])c1ccc(cc1)C(=O)N", while m05 is from series "COc1ccc(cc1)C(=O)N" ✓

For Task 1, the predicted QED for m05 is 0.5416 based on the trend of its congeneric series using polynomial fitting with degree 2.