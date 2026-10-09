# Notes for Drug-likeness Analysis Task

## Task 1: Predicting QED for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so direct measurement returns probe_unavailable. I identified that m05 belongs to a congeneric series with the scaffold COc1ccc(cc1)C(=O)N, which includes molecules m15, m24, m28, m17, and m22, differing only in the length of the N-alkyl chain.

I measured the QED of the following molecules in this series:
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

Using fit_trend with these 5 data points, I predicted the QED for m05 (chain length 11) to be 0.5416.

## Task 2: Finding a lead molecule

I searched for a molecule meeting all three criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same congeneric series as m05

From the molecular pool, I identified two groups with PSA ≥ 40:
- Group with PSA ≈ 110.02 (polyol group: OCC(O)C(O)C(O)C(=O)N...)
- Group with PSA = 72.24 (nitrobenzene group: O=[N+]([O-])c1ccc(cc1)C(=O)N...)

I measured several molecules from these groups:
- m14: QED = 0.3237 (< 0.6)
- m27: QED = 0.333 (< 0.6)
- m01: QED = 0.3187 (< 0.6)
- m09: QED = 0.5826 (< 0.6)
- m02: QED = 0.6046 (≥ 0.6) - meets all criteria!

Molecule m02 meets all requirements:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Belongs to nitrobenzene congeneric series, not the same as m05's methoxybenzene series ✓

## Summary of Measurements Used

I used all 12 of my measurement budget:
1. m15
2. m24
3. m28
4. m17
5. m22
6. m14
7. m27
8. m01
9. m09
10. m02
11. m13
12. m12