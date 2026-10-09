# Task Notes

## Task 1: Predict QED of m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to the congeneric series with scaffold "COc1ccc(cc1)C(=O)N" and has chain length 11.

I measured the QED of other molecules in the same series:
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

Using polynomial fitting with these 5 data points, I predicted the QED of m05 (chain length 11) to be 0.5416.

When attempting to measure m05 directly, the system returned "probe_unavailable" confirming that this molecule has not been synthesized.

## Task 2: Find a suitable lead molecule

Criteria:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same congeneric series as m05

From the group_by_scaffold analysis, m05 belongs to the series with scaffold "COc1ccc(cc1)C(=O)N".

I tested several molecules from other series:
- m09 (series "O=[N+]([O-])c1ccc(cc1)C(=O)N"): QED = 0.5826 (< 0.6, doesn't qualify)
- m02 (series "O=[N+]([O-])c1ccc(cc1)C(=O)N"): QED = 0.6046, psa = 72.24 (≥ 40), different series ✓

Molecule m02 meets all three criteria:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Different scaffold from m05 ✓

Therefore, m02 is the selected lead molecule for Task 2.