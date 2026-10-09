# Notes

## Task 1: Predict QED for m05

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (methoxy-benzamide with C11 alkyl chain)

m05 cannot be measured directly (probe_unavailable), so I predicted from its congeneric series.

### Congeneric series members (same scaffold `COc1ccc(cc1)C(=O)N-`, different chain lengths):
| mol_id | chain length | QED (measured) |
|--------|-------------|----------------|
| m15    | C2          | 0.7621         |
| m24    | C3          | 0.7919         |
| m28    | C4          | 0.7518         |
| m17    | C5          | 0.7498         |
| m22    | C6          | 0.7377         |
| m05    | C11         | ? (target)     |

### Trend analysis:
- QED peaks at C3 (0.7919), then decreases with increasing chain length
- From C3 to C6: slope ≈ -0.0181 QED units per carbon
- Extrapolating to C11: 0.7919 - 0.0181 × 8 = 0.6471

**Predicted QED for m05: ~0.65**

## Task 2: Find lead molecule

Criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. NOT in same congeneric series as m05

### Molecules measured for Task 2:
| mol_id | QED    | PSA    | In m05 series? | Qualifies? |
|--------|--------|--------|----------------|------------|
| m01    | 0.3187 | 110.02 | No             | No (QED < 0.6) |
| m02    | 0.6046 | 72.24  | No             | **YES**    |
| m09    | 0.5826 | 72.24  | No             | No (QED < 0.6) |

### Selected lead: m02
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` (nitro-benzamide with C3 chain)
- QED: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Different scaffold from m05 (nitro- vs methoxy-benzamide) ✓

## Budget usage:
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 units (budget exhausted)
