# Notes

## Task 1: QED of m05
- m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- m05 has never been synthesized, so measure_qed returns probe_unavailable
- Need to predict QED from its congeneric series (same scaffold, different N-alkyl chain lengths)

### Congeneric series with m05 (COc1ccc(cc1)C(=O)N- scaffold):
- m15: NCC (2C chain)
- m24: NCCC (3C chain)
- m28: NCCCC (4C chain)
- m17: NCCCCC (5C chain)
- m22: NCCCCCC (6C chain)
- m05: NCCCCCCCCCCC (12C chain) - TARGET

## Task 2: Find molecule with:
1. QED ≥ 0.6
2. psa ≥ 40
3. NOT in same congeneric series as m05

### Molecules with psa ≥ 40 (from pool.tsv):
- m01: psa=110.02 (sugar-amide series)
- m03: psa=110.02 (sugar-amide series)
- m07: psa=110.02 (sugar-amide series)
- m14: psa=110.02 (sugar-amide series)
- m18: psa=110.02 (sugar-amide series)
- m19: psa=110.02 (sugar-amide series)
- m21: psa=110.02 (sugar-amide series)
- m25: psa=110.02 (sugar-amide series)
- m27: psa=110.02 (sugar-amide series)
- m02: psa=72.24 (nitro-amide series)
- m06: psa=72.24 (nitro-amide series)
- m09: psa=72.24 (nitro-amide series)
- m10: psa=72.24 (nitro-amide series)
- m12: psa=72.24 (nitro-amide series)
- m29: psa=72.24 (nitro-amide series)
- m31: psa=72.24 (nitro-amide series)
- m32: psa=72.24 (nitro-amide series)
- m33: psa=72.24 (nitro-amide series)

### Molecules to exclude (same series as m05 - COc1ccc(cc1)C(=O)N-):
m15, m17, m22, m24, m28, m05

## Budget: 12 measurements total

## Plan:
1. Measure some molecules in m05's congeneric series to establish QED trend
2. Measure molecules with psa ≥ 40 from OTHER series to find one with QED ≥ 0.6

## Measurements taken (12/12):
| mol_id | QED | psa | Series |
|--------|-----|-----|--------|
| m15 | 0.7621 | 38.33 | m05 congeneric (2C) |
| m24 | 0.7919 | 38.33 | m05 congeneric (3C) |
| m28 | 0.7518 | 38.33 | m05 congeneric (4C) |
| m17 | 0.7498 | 38.33 | m05 congeneric (5C) |
| m22 | 0.7377 | 38.33 | m05 congeneric (6C) |
| m01 | 0.3187 | 110.02 | sugar-amide |
| m14 | 0.3237 | 110.02 | sugar-amide |
| m09 | 0.5826 | 72.24 | nitro-amide |
| m12 | 0.4709 | 72.24 | nitro-amide |
| m02 | 0.6046 | 72.24 | nitro-amide ✓ |
| m32 | 0.4683 | 72.24 | nitro-amide |
| m33 | 0.4591 | 72.24 | nitro-amide |

## Task 1: Predicting m05 QED
Congeneric series trend (chain length vs QED):
- 2C (m15): 0.7621
- 3C (m24): 0.7919 (peak)
- 4C (m28): 0.7518
- 5C (m17): 0.7498
- 6C (m22): 0.7377
- 12C (m05): ?

Linear extrapolation from 3C-6C trend: slope ≈ -0.0181 per carbon
Predicted QED for 12C: 0.7919 - 0.0181*(12-3) ≈ 0.629

Considering non-linear penalty for high rotb (12) and high logP (4.956):
**Predicted m05 QED ≈ 0.58**

## Task 2: Selected molecule
**m02** meets all criteria:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. NOT in m05's congeneric series (different scaffold: nitro-phenyl vs methoxy-phenyl) ✓

SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
