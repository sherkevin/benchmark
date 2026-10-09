# Notes

## Task 1: Predict QED for m05
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (305.46, alogp=4.956, psa=38.33, rotb=12, arom=1)
- m05 is not synthesized, so measure_qed returns probe_unavailable
- Need to predict QED from congeneric series (same scaffold, different N-alkyl chain length)

### m05 Congeners (Series 1: COc1ccc(cc1)C(=O)N)
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Fit polynomial (degree 2) with x_query=11:
- Predicted QED for m05 = 0.5416

## Task 2: Find a lead molecule
Constraints:
1. measured QED >= 0.6
2. psa >= 40
3. NOT in same congeneric series as m05

### Measurements for candidates
| mol_id | series | psa   | QED    | Meets criteria? |
|--------|--------|-------|--------|-----------------|
| m09    | 2      | 72.24 | 0.5826 | No (QED < 0.6)  |
| m02    | 2      | 72.24 | 0.6046 | YES!            |
| m12    | 2      | 72.24 | 0.4709 | No              |
| m14    | 3      | 110.02| 0.3237 | No              |
| m27    | 3      | 110.02| 0.333  | No              |
| m01    | 3      | 110.02| 0.3187 | No              |

### Selected lead: m02
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Series 2 (nitro series), NOT m05's series ✓

## Summary
- Task 1: m05 predicted QED = 0.5416
- Task 2: lead_mol_id = m02, lead_qed_measured = 0.6046
