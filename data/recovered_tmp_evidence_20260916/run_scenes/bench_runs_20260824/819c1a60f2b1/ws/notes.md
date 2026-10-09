Medicinal Chemistry Task Notes

Task 1: Predicting QED of m05
- m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) could not be measured directly as it has not been synthesized
- Identified m05's congeneric series using group_by_scaffold: scaffold "COc1ccc(cc1)C(=O)N" with varying N-alkyl chain lengths
- Members of the series include: m15, m24, m28, m17, m22, and m05 itself
- Measured QED of 5 compounds in the series:
  - m15 (chain_len=2): QED = 0.7621
  - m24 (chain_len=3): QED = 0.7919
  - m28 (chain_len=4): QED = 0.7518
  - m17 (chain_len=5): QED = 0.7498
  - m22 (chain_len=6): QED = 0.7377
- Used fit_trend to predict QED of m05 (chain_len=11): 0.5416

Task 2: Finding a suitable lead molecule
- Requirements: QED ≥ 0.6, psa ≥ 40, not in same series as m05
- Tested multiple molecules from other series:
  - m01: QED = 0.3187 (< 0.6)
  - m02: QED = 0.6046 (≥ 0.6), psa = 72.24 (≥ 40), different series ✓
  - m14: QED = 0.3237 (< 0.6)
  - m03: QED = 0.3282 (< 0.6)
  - m12: QED = 0.4709 (< 0.6)
  - m09: QED = 0.5826 (< 0.6)
  - m32: QED = 0.4683 (< 0.6)
- Selected m02 as it met all three criteria

Final selections:
- Task 1: QED of m05 = 0.5416
- Task 2: Lead molecule = m02 with measured QED = 0.6046