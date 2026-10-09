# Notes for Molecular Design Task

## Analysis
- Identified 4 congeneric series based on shared scaffolds
- m05 belongs to the COc1ccc(cc1)C(=O)N scaffold series with varying N-alkyl chain lengths
- Members of m05's series: m15, m24, m28, m17, m22, m05

## Task 1 - Predicting QED for m05
- Measured QED values for other members of m05's series:
  - m15 (chain_len=2): QED = 0.7621
  - m24 (chain_len=3): QED = 0.7919
  - m28 (chain_len=4): QED = 0.7518
  - m17 (chain_len=5): QED = 0.7498
  - m22 (chain_len=6): QED = 0.7377
- Used fit_trend to predict m05's QED (chain_len=11): 0.5416

## Task 2 - Finding optimal molecule
- Needed: QED ≥ 0.6, psa ≥ 40, not in same series as m05
- Explored molecules from other series:
  - Hydroxyl series: Most had low QED values (< 0.6)
  - Nitrobenzene series: Found m02 with QED = 0.6046, psa = 72.24
- Selected m02 which meets all criteria:
  - QED = 0.6046 ≥ 0.6 ✓
  - psa = 72.24 ≥ 40 ✓
  - Not in same series as m05 ✓

## Budget Usage
- Used 12 out of 12 measurements