# Task Completion Notes

## Task 1: Predicting QED for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so direct measurement returns probe_unavailable. To predict its QED, I measured 5 other molecules from the same congeneric series (same scaffold COc1ccc(cc1)C(=O)N, differing only by N-alkyl chain length):

- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

Using these 5 points, I fitted a polynomial trend to predict m05's QED (chain length 11), which resulted in a predicted QED of 0.5416.

## Task 2: Finding a suitable lead molecule

For Task 2, I needed to find a molecule satisfying:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same congeneric series as m05

Based on the molecular structures, I identified that molecules from the first series (OCC(O)C(O)C(O)C(=O)N) and second series (O=[N+]([O-])c1ccc(cc1)C(=O)N) have PSA values of 110.02 and 72.24 respectively, both ≥ 40. The other two series have PSA values below 40.

I measured several molecules from both series:
- First series (PSA=110.02): m14, m27, m01, m19 - all had QED values below 0.6 (around 0.32)
- Second series (PSA=72.24): m09 (QED=0.5826), m02 (QED=0.6046), m12 (QED=0.4709)

Molecule m02 met all three criteria:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓  
- Belongs to the NO2-substituted series, not the OCH3-substituted series as m05 ✓

## Budget Usage
Total measurements used: 12/12 budget units