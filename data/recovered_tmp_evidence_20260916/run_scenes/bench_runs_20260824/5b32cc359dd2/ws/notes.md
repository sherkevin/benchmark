# Notes for QED Prediction Task

## Job 1: Predict QED of m05

### Step 1: Identify congeners of m05 (same scaffold)

Used describe_structure on m05 and potential congeners. All molecules with scaffold `COc1ccc(cc1)C(=O)N` (para-methoxy-benzamide) are in the same series:

| Mol ID | Chain Length | SMILES |
|--------|--------------|--------|
| m15    | 2            | COc1ccc(cc1)C(=O)NCC |
| m24    | 3            | COc1ccc(cc1)C(=O)NCCC |
| m28    | 4            | COc1ccc(cc1)C(=O)NCCCC |
| m17    | 5            | COc1ccc(cc1)C(=O)NCCCCC |
| m22    | 6            | COc1ccc(cc1)C(=O)NCCCCCC |
| m05    | 11 (target)  | COc1ccc(cc1)C(=O)NCCCCCCCCCCC |

Congeners in pool: **m15, m24, m28, m17, m22** (5 molecules)

### Step 2: Measure QED of congeners

Measured all 5 congeners (used 5 of 12 budget):

| Chain Length (x) | QED (y) | Mol ID |
|------------------|---------|--------|
| 2                | 0.7621  | m15    |
| 3                | 0.7919  | m24    |
| 4                | 0.7518  | m28    |
| 5                | 0.7498  | m17    |
| 6                | 0.7377  | m22    |

### Step 3: Fit trend and extrapolate

The data shows QED peaks around chain length 3, then decreases as chain length increases.

**Quadratic fit (degree=2):**
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188
- Prediction at x=11: **0.5416**

**Linear fit (degree=1) for comparison:**
- RMSE: 0.01308
- Prediction at x=11: 0.695

The quadratic fit has lower RMSE (0.01188 vs 0.01308), so it's the better model.

**Final prediction for m05 QED: 0.5416**

---

## Job 2: Pick a backup lead compound

### Constraints:
1. measured QED >= 0.6
2. psa >= 40 (free check)
3. not in m05 series

### Step 1: Screen by psa >= 40

18 molecules pass the psa >= 40 screen:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

Note: The m05 series molecules (m15, m17, m22, m24, m28) all have psa=38.33 < 40, so they're automatically excluded by constraint 2.

### Step 2: Measure QED of candidates (excluding m05 series)

Measurements (used 4 more of budget, total 9 used):
- m01: QED = 0.3187 (fails, < 0.6)
- m02: QED = 0.6046 (PASS!)
- m09: QED = 0.5826 (fails, < 0.6)
- m14: QED = 0.3237 (fails, < 0.6)

### Step 3: Verify m02 meets all constraints

1. **QED >= 0.6**: 0.6046 >= 0.6 ✓
2. **psa >= 40**: 72.24 >= 40 ✓
3. **Not in m05 series**: Scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide), different from m05's `COc1ccc(cc1)C(=O)N` (methoxy-benzamide) ✓

**Selected lead: m02**

---

## Summary

- **target_qed**: 0.5416 (predicted via quadratic fit on congeneric series)
- **lead_mol_id**: m02
- **lead_qed_measured**: 0.6046
- **congener_ids**: ["m15", "m17", "m22", "m24", "m28"]

Budget used: 9 of 12 measurements
