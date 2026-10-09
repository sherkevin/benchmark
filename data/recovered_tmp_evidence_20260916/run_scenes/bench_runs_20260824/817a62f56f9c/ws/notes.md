# Notes — QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Identifying the congeneric series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. Using `describe_structure`, its scaffold is `COc1ccc(cc1)C(=O)N` (methoxybenzamide) with chain_len = 11.

Pool molecules sharing this scaffold (confirmed via `describe_structure`):

| mol_id | chain_len | QED (measured) |
|--------|-----------|-----------------|
| m15    | 2         | 0.7621          |
| m24    | 3         | 0.7919          |
| m28    | 4         | 0.7518          |
| m17    | 5         | 0.7498          |
| m22    | 6         | 0.7377          |
| m05    | 11        | (never synthesized — predict) |

Congener IDs: m15, m24, m28, m17, m22

### Observed trend

QED rises from chain=2 (0.7621) to a peak at chain=3 (0.7919), then steadily declines through chain=4 (0.7518), chain=5 (0.7498), and chain=6 (0.7377). The relationship is clearly non-linear — a straight line misses the initial rise.

### Fitted trend and extrapolation

**Quadratic fit on all 5 points (degree=2):**
- f(x) = 0.74932 + 0.017024·x − 0.003264·x²
- RMSE on fitted points = 0.01188
- Predicted QED at chain=11: **0.5416**

**Comparison with linear fit (degree=1):**
- f(x) = 0.79502 − 0.00909·x
- RMSE = 0.01308
- Predicted QED at chain=11: 0.6950

The quadratic is preferred because:
1. The task says "do not assume the relationship is a straight line"
2. The data clearly shows curvature (peak at chain 3, then decline)
3. The quadratic has a lower RMSE (0.01188 vs 0.01308)
4. The negative quadratic coefficient captures the physical reality that longer chains increasingly penalize drug-likeness

**Final prediction for m05 QED: 0.5416**

### Budget spent on Job 1: 5 measurements (m15, m24, m28, m17, m22)

---

## Job 2: Pick a backup lead compound

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. NOT in the same series as m05

### Pre-filtering (free checks)

Used `screen_candidates()` to find all molecules with PSA ≥ 40. This returned 18 molecules from two series:
- Sugar acid series (PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrobenzene series (PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

None of these are in the m05 series (which has PSA=38.33, below the threshold), so constraint 3 is automatically satisfied for all.

### Measurements for Job 2 (7 remaining from 12 total budget)

Focused on the nitrobenzene series since their alogp values (1.3–4.5) are more in drug-like range than the sugar acid series (alogp -2.8 to +0.3):

| mol_id | chain_len | alogp  | QED (measured) | QED ≥ 0.6? |
|--------|-----------|--------|-----------------|------------|
| m09    | 2         | 1.345  | 0.5826          | No         |
| m02    | 3         | 1.735  | 0.6046          | **Yes**    |
| m12    | 4         | 2.125  | 0.4709          | No         |
| m32    | 5         | 2.515  | 0.4683          | No         |
| m33    | 6         | 2.905  | 0.4591          | No         |
| m29    | 7         | 3.295  | 0.4446          | No         |
| m31    | 8         | 3.685  | 0.4252          | No         |

The nitrobenzene series QED peaks at short chain lengths and declines monotonically. Only m02 (chain=3) achieves QED ≥ 0.6.

### Selected lead: m02

- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Series: nitrobenzene amide (NOT m05's methoxybenzamide series ✓)

### Budget spent on Job 2: 7 measurements (m09, m02, m12, m32, m33, m29, m31)

### Total budget used: 12/12 measurements
