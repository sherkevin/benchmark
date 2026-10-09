# Notes

## Task 1: Predict QED for m05

m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) cannot be measured directly (probe_unavailable).
It belongs to the scaffold `COc1ccc(cc1)C(=O)N` with chain length 11.

### Measurements on m05's congeneric series:
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Polynomial fit (degree=2):
- Fitted trend through points (chain_len, QED)
- Predicted QED for chain_len=11 (m05): **0.5416**

## Task 2: Find lead molecule

Constraints:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in same congeneric series as m05 (i.e., not scaffold `COc1ccc(cc1)C(=O)N`)

### Scaffold analysis:
- m05's scaffold (COc1ccc...): psa=38.33 - all members fail constraint 2 anyway
- Nitro scaffold (O=[N+]([O-])c1ccc...): psa=72.24 - candidates
- Sugar-like scaffold (OCC(O)C(O)C(O)C(=O)N): psa=110.02 - candidates
- Biphenyl scaffold (c1ccc(cc1)-c1ccc...): psa=29.1 - all fail constraint 2

### Measurements for candidates:
| mol_id | scaffold | psa | QED | Meets criteria? |
|--------|----------|-----|-----|-----------------|
| m02    | Nitro    | 72.24 | 0.6046 | YES ✓ |
| m09    | Nitro    | 72.24 | 0.5826 | No (QED < 0.6) |
| m14    | Sugar    | 110.02 | 0.3237 | No (QED < 0.6) |
| m27    | Sugar    | 110.02 | 0.333 | No (QED < 0.6) |
| m01    | Sugar    | 110.02 | 0.3187 | No (QED < 0.6) |

### Selected lead: m02
- QED: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05 ✓)

## Budget used: 10 out of 12 measurements
