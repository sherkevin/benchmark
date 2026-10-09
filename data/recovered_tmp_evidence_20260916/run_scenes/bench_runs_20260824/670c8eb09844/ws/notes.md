# Notes for Molecular Design Task

## Initial Analysis
- Need to determine the QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) which cannot be directly measured
- Need to find a molecule that meets three criteria:
  1. Measured QED ≥ 0.6
  2. PSA ≥ 40
  3. Not in the same congeneric series as m05

## Scaffolding Information
- m05 belongs to the first group with scaffold "COc1ccc(cc1)C(=O)N" and has chain length 11
- Congeneric series for m05: m15(chain_len=2), m24(chain_len=3), m28(chain_len=4), m17(chain_len=5), m22(chain_len=6), m05(chain_len=11)
- Other scaffolds: "O=[N+]([O-])c1ccc(cc1)C(=O)N", "OCC(O)C(O)C(O)C(=O)N", "c1ccc(cc1)-c1ccc(cc1)C(=O)N"

## Molecules in m05's Congeneric Series
- m15: chain_len=2, PSA=38.33
- m24: chain_len=3, PSA=38.33
- m28: chain_len=4, PSA=38.33
- m17: chain_len=5, PSA=38.33
- m22: chain_len=6, PSA=38.33
- m05: chain_len=11, PSA=38.33

## QED measurements for m05's congeneric series
- m15: QED = 0.7621
- m24: QED = 0.7919
- m28: QED = 0.7518
- m17: QED = 0.7498
- m22: QED = 0.7377
- Using fit_trend with chain lengths [2,3,4,5,6] and corresponding QEDs, predicting for chain length 11: QED = 0.5416

## Task 2 - Finding a suitable molecule
Criteria: QED ≥ 0.6, PSA ≥ 40, not in same series as m05
- Molecules from "O=[N+]([O-])c1ccc(cc1)C(=O)N" scaffold: all have PSA = 72.24
- Molecules from "OCC(O)C(O)C(O)C(=O)N" scaffold: all have PSA = 110.02
- Both of these scaffolds are different from m05's scaffold "COc1ccc(cc1)C(=O)N"

Tested m09: QED = 0.5826 (< 0.6) - doesn't meet criteria
Tested m02: QED = 0.6046 (≥ 0.6), PSA = 72.24 (≥ 40), different scaffold - meets all criteria!