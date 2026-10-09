# Medicinal Chemistry Task Notes

## Overview
- Total molecules: 33
- Budget: 17 units (9 used so far for listing all pages, 8 remaining)
- Target: m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC)
- Need to: 1) Predict QED for m05, 2) Find molecule with QED ≥ 0.6, PSA ≥ 40, not in same series as m05

## Molecule Series Analysis

### Methoxy-phenyl series (same as m05):
- m15: COc1ccc(cc1)C(=O)NCC (2C) - PSA: 38.33
- m24: COc1ccc(cc1)C(=O)NCCC (3C) - PSA: 38.33
- m28: COc1ccc(cc1)C(=O)NCCCC (4C) - PSA: 38.33
- m17: COc1ccc(cc1)C(=O)NCCCCC (5C) - PSA: 38.33
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6C) - PSA: 38.33
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11C) - PSA: 38.33

### Nitro-phenyl series:
- m09, m12, m32, m33, m29, m31, m10, m06, m02
- All have PSA = 72.24

### Biphenyl series:
- m13, m11, m04, m30, m20, m26, m08, m16, m23
- All have PSA = 29.1

### Polyol series:
- m14, m27, m01, m19, m21, m18, m07, m25, m03
- All have PSA = 110.02

## Task 1 Strategy
Need to predict QED for m05. Should measure QED for some methoxy-phenyl series compounds to establish trend vs alkyl chain length. Then extrapolate to 11C chain.

### Measurements for Methoxy-phenyl series:
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

Pattern: Peak at 3C (0.7919), then gradual decrease with chain length.
Target: m05 has 11C chain. Need to extrapolate.

### Other measurements:
- m01 (polyol 4C): QED = 0.3187 (too low)
- m02 (nitro-phenyl 3C): QED = 0.6046 (qualifies for Task 2)
- m09 (nitro-phenyl 2C): QED =323 0.5826 (below threshold)

### Budget:
Total used: 17/17 units exhausted.

## Task 2 Strategy
Need molecule with:
1. QED ≥ 0.6
2. PSA ≥ 40
3. Not in methoxy-phenyl series

PSA ≥ 40 eliminates biphenyl series (all PSA=29.1). So candidates must be from nitro-phenyl (PSA=72.24) or polyol (PSA=110.02) series.

Will measure QED for promising candidates.

## Final Decisions

### Task 1: Predicted QED for m05
Based on trend in methoxy-phenyl series:
- Chain lengths 2C-6C show QED values: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377
- Trend shows decrease after 3C peak
- For 11C chain (m05), predicted QED = 0.685
- Rationale: Longer alkyl chains increase lipophilicity (alogp), which typically lowers QED. Extrapolating from 4C-6C trend gives ~0.702, but 11C is much longer so adjusted downward.

### Task 2: Selected molecule
- Chosen: m02
- Reason: 
  1. Measured QED = 0.6046 ≥ 0.6
  2. PSA = 72.24 ≥ 40
  3. Not in methoxy-phenyl series (nitro-phenyl scaffold)
- Alternative candidates not measured due to budget constraints, but m02 satisfies all criteria.