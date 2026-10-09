# Notes for QED Prediction Task

## Overview
Task has two parts:
1. Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) - has never been synthesized, so need to use congeneric series
2. Find a molecule that satisfies: QED ≥ 0.6, psa ≥ 40, and not in same series as m05

Budget: 12 measurements total for both tasks

## Initial Observations
Looking at data/pool.tsv:
- m05 has: mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1
- We need to find molecules with psa ≥ 40 for Task 2
- Different scaffolds/types visible: 
  - OCC(O)C(O)C(O)C(=O)N... (polyol series)
  - O=[N+]([O-])c1ccc(cc1)C(=O)N... (nitrophenyl series)
  - c1ccc(cc1)-c1ccc(cc1)C(=O)N... (biphenyl series)
  - COc1ccc(cc1)C(=O)N... (methoxyphenyl series - this is m05's series)

## Step 1: Group by scaffold
Used group_by_scaffold():
- 4 scaffolds total
- m05 is in "COc1ccc(cc1)C(=O)N" series with chain_len=11
- Other members in this series: m15 (2), m24 (3), m28 (4), m17 (5), m22 (6)

## Step Card 2: Measured QED for m05 series members (5 measurements used)
1. m15 (chain_len=2): QED = 0.7621
2. m24 (chain_len=3): QED = 0.7919  
3. m28 (chain_len=4): QED = 0.7518
4. m17 (chain_len=5): QED = 0.7498
5. m22 (chain_len=6): QED = 0.7377

Budget remaining: 7 measurements (12 total - 5 used)

Pattern: QED seems to decrease as chain length increases beyond 3 carbons.

## Step 3: Predict m05 QED (chain_len=11)
Tried different polynomial fits:
- Linear fit (degree=1): predicts 0.695
- Quadratic fit (degree=2): predicts 0.5416 (more realistic given long chain)
- Cubic fit (degree=3): predicts 2.1323 (impossible, overfitting)

Quadratic seems most reasonable given the decreasing trend.

## Step 4: Task 2 - Find molecule with QED ≥ 0.6, psa ≥ 40, not in m05 series

### Candidate analysis:
- psa ≥ 40 molecules: polyol series (psa=110.02) and nitrophenyl series (psa=72.24)
- Both series are not in m05's methoxyphenyl series

### Measurements:
1. m14 (polyol, chain_len=2): QED = 0.3237
2. m27 (polyol, chain_len=3): QED = 0.333
3. m01 (polyol, chain_len=4): QED = 0.3187
4. m09 (nitrophenyl, chain_len=2): QED = 0.5826 (close but < 0.6)
5. m02 (nitrophenyl, chain_len=3): QED = 0.6046 ✓ (≥ 0.6!)

### Verification:
- m02: QED = 0.6046 (measured)
- psa = 72.24 (from data, ≥ 40)
- Series: nitrophenyl (O=[N+]([O-])c1ccc(cc1)C(=O)N) - not m05's series

Total measurements used: 10 of 12

## Conclusion:
For Task 1: Predict m05 QED as 0.5416 (quadratic fit)
For Task 2: Select m02 with measured QED 0.6046

## Final Answer:
- target_qed: 0.5416 (predicted QED for m05)
- lead_mol_id: "m02" (selected molecule)
- lead_qed_measured: 0.6046 (measured QED for m02)

Budget used: 10 of 12 measurements.